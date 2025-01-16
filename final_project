import simplegui
import random
import time

# Global variables
inventory = []
health = 100
score = 0
current_room = "Starting Room"
time_left = 10  # Time to reach the ball (in seconds)
game_over = False

# SimpleGUI setup
frame = simplegui.create_frame("Dog Adventure Game", 500, 400)

# Labels for game information
room_label = frame.add_label("You are in the starting room. Find the ball!")
inventory_label = frame.add_label("Inventory: ")
health_label = frame.add_label("Health: " + str(health))
score_label = frame.add_label("Score: " + str(score))
timer_label = frame.add_label("Time Left: " + str(time_left))

# Define Room Descriptions
rooms = {
    "Starting Room": "You are in the house. The door to the yard is open.",
    "Yard": "You are in the yard. You spot the ball!",
    "Park": "You are at the park. Run towards the ball!"
}

# Timer to decrease time_left
def timer_handler():
    global time_left, game_over
    if time_left > 0:
        time_left -= 1
        timer_label.set_text(f"Time Left: {time_left}")
    else:
        game_over = True
        room_label.set_text("You ran out of time! Game Over!")
        update_gui()

# Random Event: Encountering obstacles
def random_event():
    event = random.choice(["found a bone", "found a treat", "tripped over something"])
    if event == "found a bone":
        inventory.append("Bone")
        room_label.set_text("You found a bone! It boosts your speed!")
    elif event == "found a treat":
        inventory.append("Treat")
        room_label.set_text("You found a treat! It restores health!")
    else:
        room_label.set_text("You tripped over something! Lose 5 health.")
        lose_health(5)
    update_gui()

# Button actions
def go_to_yard():
    global current_room
    if current_room == "Starting Room":
        current_room = "Yard"
        room_label.set_text(rooms[current_room])
    update_gui()

def go_to_park():
    global current_room
    if current_room == "Yard":
        current_room = "Park"
        room_label.set_text(rooms[current_room])
        start_timer()
    update_gui()

def start_timer():
    global time_left, game_over
    time_left = 10  # Reset the timer when the dog is at the park
    game_over = False
    timer_label.set_text(f"Time Left: {time_left}")
    timer.start()

def catch_ball():
    global score, time_left, game_over
    if current_room == "Park" and not game_over:
        if time_left > 0:
            score += 20
            room_label.set_text(f"Good job! You caught the ball! Score: {score}")
        else:
            score -= 10
            room_label.set_text(f"You missed the ball! Lost 10 points. Score: {score}")
        update_gui()

def use_item():
    global health
    if "Treat" in inventory:
        inventory.remove("Treat")
        health += 10
        health_label.set_text("Health: " + str(health))
        room_label.set_text("You used a treat! Health restored.")
    else:
        room_label.set_text("You have no treats!")

def lose_health(amount):
    global health
    health -= amount
    if health <= 0:
        game_over = True
        room_label.set_text("You lost all health! Game Over!")
    update_gui()

def update_gui():
    inventory_label.set_text("Inventory: " + ", ".join(inventory))
    health_label.set_text("Health: " + str(health))
    score_label.set_text("Score: " + str(score))

# Create buttons for actions
frame.add_button("Go to Yard", go_to_yard, 150)
frame.add_button("Go to Park", go_to_park, 150)
frame.add_button("Catch the Ball", catch_ball, 150)
frame.add_button("Use Item", use_item, 150)
frame.add_button("Random Event", random_event, 150)

# Start the game
frame.start()

# Start the timer
timer = simplegui.create_timer(1000, timer_handler)

# Draw the dog and ball
dog_x = 50
dog_y = 300
ball_x = 450
ball_y = 300

def draw(canvas):
    global dog_x, time_left, game_over

    # Draw the dog (simple rectangle)
    canvas.draw_polygon([(dog_x, dog_y), (dog_x + 30, dog_y), (dog_x + 30, dog_y - 30), (dog_x, dog_y - 30)], 1, "Black", "Brown")

    # Draw the ball (simple circle)
    canvas.draw_circle((ball_x, ball_y), 15, 1, "Black", "Red")

    # If the dog is running, move it towards the ball
    if current_room == "Park" and not game_over:
        if dog_x < ball_x - 30:
            dog_x += 5
        if dog_x >= ball_x - 30:  # If the dog catches the ball
            canvas.draw_text("You caught the ball!", (200, 150), 30, "White")
        
    elif game_over:
        canvas.draw_text("Game Over!", (200, 150), 30, "Red")

# Set the draw handler
frame.set_draw_handler(draw)
