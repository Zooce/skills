# Odin

## Scenario

A room is reserved for a stretch of time. The reservation goes on a list. A second reservation for that room is refused when the times overlap.

| Step | Input | Output or Effect |
| --- | --- | --- |
| `reserve` | the list and a reservation | the reservation is on the list, or it is refused |
| `cancel` | the list and a reservation id | that reservation is gone from the list |

A reservation is an id, a room id, a start time, and an end time. The start is included. The end is not. 9:00 to 11:00 and 11:00 to 13:00 can share a room. 9:00 to 11:00 and 10:00 to 12:00 cannot.

Checking the overlap and adding the reservation stay in `reserve`. The overlap is only used there.

This fits in one file. Split the file when a smaller piece is easier to find.

```odin
package rooms

import "core:testing"
import "core:time"

Reservation_Id :: distinct u32
Room_Id :: distinct u32

Reservation :: struct {
	id:      Reservation_Id,
	room_id: Room_Id,
	start:   time.Time,
	end:     time.Time,
}

Reservations :: struct {
	items: [16]Reservation,
	count: int,
}

reserve :: proc(list: ^Reservations, incoming: Reservation) -> bool {
	if time.diff(incoming.start, incoming.end) <= 0 {
		return false
	}

	// Same room, overlapping time: refuse.
	for i in 0 ..< list.count {
		existing := list.items[i]
		if existing.room_id != incoming.room_id {
			continue
		}
		if time.diff(incoming.start, existing.end) > 0 && time.diff(existing.start, incoming.end) > 0 {
			return false
		}
	}

	// Add the reservation.
	if list.count >= len(list.items) {
		return false
	}
	list.items[list.count] = incoming
	list.count += 1
	return true
}

cancel :: proc(list: ^Reservations, id: Reservation_Id) -> bool {
	for i in 0 ..< list.count {
		if list.items[i].id == id {
			list.count -= 1
			for j := i; j < list.count; j += 1 {
				list.items[j] = list.items[j + 1]
			}
			return true
		}
	}
	return false
}

at :: proc(hour: int) -> time.Time {
	t, ok := time.datetime_to_time(2026, 10, 3, hour, 0, 0)
	assert(ok)
	return t
}

@(test)
back_to_back_times_on_one_room :: proc(t: ^testing.T) {
	list: Reservations
	testing.expect(t, reserve(&list, Reservation{id = 1, room_id = 1, start = at(9), end = at(11)}))
	testing.expect(t, reserve(&list, Reservation{id = 2, room_id = 1, start = at(11), end = at(13)}))
	testing.expect_value(t, list.count, 2)
}

@(test)
overlap_on_same_room_is_refused :: proc(t: ^testing.T) {
	list: Reservations
	testing.expect(t, reserve(&list, Reservation{id = 1, room_id = 1, start = at(9), end = at(11)}))
	testing.expect(t, !reserve(&list, Reservation{id = 2, room_id = 1, start = at(10), end = at(12)}))
	testing.expect_value(t, list.count, 1)
}

@(test)
same_times_on_two_rooms :: proc(t: ^testing.T) {
	list: Reservations
	testing.expect(t, reserve(&list, Reservation{id = 1, room_id = 1, start = at(9), end = at(11)}))
	testing.expect(t, reserve(&list, Reservation{id = 2, room_id = 2, start = at(9), end = at(11)}))
	testing.expect_value(t, list.count, 2)
}

@(test)
cancel_frees_the_time :: proc(t: ^testing.T) {
	list: Reservations
	testing.expect(t, reserve(&list, Reservation{id = 1, room_id = 1, start = at(9), end = at(11)}))
	testing.expect(t, cancel(&list, 1))
	testing.expect_value(t, list.count, 0)
	testing.expect(t, !cancel(&list, 1))
	testing.expect(t, reserve(&list, Reservation{id = 2, room_id = 1, start = at(9), end = at(11)}))
	testing.expect_value(t, list.count, 1)
}
```
