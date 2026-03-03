from collections import deque

# State representation:
# (monkey_position, box_position, monkey_on_box, has_banana)

initial_state = ("A", "B", False, False)
goal_state = ("C", "C", True, True)  # Monkey under banana, on box, has banana

# Banana is at position C
banana_position = "C"

def get_next_states(state):
    monkey_pos, box_pos, on_box, has_banana = state
    next_states = []

    if has_banana:
        return next_states

    # Move monkey
    for position in ["A", "B", "C"]:
        if monkey_pos != position and not on_box:
            next_states.append((position, box_pos, False, False))

    # Push box (only if monkey and box are at same position)
    if monkey_pos == box_pos and not on_box:
        for position in ["A", "B", "C"]:
            if box_pos != position:
                next_states.append((position, position, False, False))

    # Climb box
    if monkey_pos == box_pos and not on_box:
        next_states.append((monkey_pos, box_pos, True, False))

    # Grasp banana
    if monkey_pos == banana_position and box_pos == banana_position and on_box:
        next_states.append((monkey_pos, box_pos, True, True))

    return next_states


def bfs():
    queue = deque([(initial_state, [])])
    visited = set()

    while queue:
        state, path = queue.popleft()

        if state in visited:
            continue
        visited.add(state)

        if state[3]:  # has_banana == True
            return path + [state]

        for next_state in get_next_states(state):
            queue.append((next_state, path + [state]))

    return None


solution = bfs()

if solution:
    print("Solution found:\n")
    for step in solution:
        print(step)
else:
    print("No solution found.")
