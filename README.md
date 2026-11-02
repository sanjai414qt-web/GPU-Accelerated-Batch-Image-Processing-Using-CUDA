Open Google Colab.



Sign in with your Google account.



Click New Notebook.



Copy the code below into the first code cell.



Click the ▶ Run button.



Wait for the simulation and output to appear.

```

import random
import math
import matplotlib.pyplot as plt

# Decentralized Multi-Agent Coordination
# Autonomous Logistics Simulation

random.seed(42)

NUM_AGENTS = 4
NUM_DELIVERIES = 8
AREA_SIZE = 100

# Create autonomous agents
agents = {}

for i in range(NUM_AGENTS):
    agents[f"Drone_{i+1}"] = {
        "position": [
            random.randint(0, AREA_SIZE),
            random.randint(0, AREA_SIZE)
        ],
        "battery": 100,
        "deliveries": 0
    }

# Generate delivery locations
deliveries = []

for i in range(NUM_DELIVERIES):
    deliveries.append({
        "id": i+1,
        "location": [
            random.randint(0, AREA_SIZE),
            random.randint(0, AREA_SIZE)
        ],
        "assigned": False
    })

# Distance calculation
def calculate_distance(pos1, pos2):
    return math.sqrt(
        (pos1[0] - pos2[0])**2 +
        (pos1[1] - pos2[1])**2
    )

# Distributed task allocation
print("DECENTRALIZED LOGISTICS SIMULATION")
print("----------------------------------")

for delivery in deliveries:

    best_agent = None
    minimum_distance = float("inf")

    # Each available agent evaluates the task locally
    for agent_name, agent in agents.items():

        if agent["battery"] <= 10:
            continue

        distance = calculate_distance(
            agent["position"],
            delivery["location"]
        )

        if distance < minimum_distance:
            minimum_distance = distance
            best_agent = agent_name

    if best_agent is not None:

        agent = agents[best_agent]

        print(
            f"Delivery {delivery['id']} assigned to "
            f"{best_agent} | Distance: {minimum_distance:.2f}"
        )

        # Simulate delivery movement
        old_position = agent["position"]
        destination = delivery["location"]

        # Update agent position
        agent["position"] = destination

        # Battery consumption
        battery_used = min(minimum_distance * 0.5, 90)
        agent["battery"] -= battery_used

        agent["deliveries"] += 1
        delivery["assigned"] = True

        print(
            f"{best_agent} completed Delivery "
            f"{delivery['id']} | Battery: "
            f"{agent['battery']:.2f}%"
        )

    else:
        print(f"Delivery {delivery['id']} could not be assigned.")

# Results
print("\nSIMULATION RESULTS")
print("-----------------")

total_completed = sum(
    1 for d in deliveries if d["assigned"]
)

for agent_name, agent in agents.items():
    print(
        f"{agent_name}: Deliveries = "
        f"{agent['deliveries']}, Battery = "
        f"{agent['battery']:.2f}%"
    )

print(f"\nTotal deliveries completed: {total_completed}")
print(f"Total deliveries requested: {NUM_DELIVERIES}")
print(f"Delivery success rate: "
      f"{(total_completed / NUM_DELIVERIES) * 100:.2f}%")

# Visualization
plt.figure(figsize=(9, 7))

for agent_name, agent in agents.items():
    x, y = agent["position"]
    plt.scatter(x, y, marker="^", s=150, label=agent_name)

for delivery in deliveries:
    x, y = delivery["location"]
    plt.scatter(x, y, marker="o", s=70, color="green")

plt.title("Decentralized Multi-Agent Logistics Simulation")
plt.xlabel("X Coordinate")
plt.ylabel("Y Coordinate")
plt.xlim(0, AREA_SIZE)
plt.ylim(0, AREA_SIZE)
plt.grid(True)
plt.legend()
plt.show()
```
<img width="1293" height="692" alt="Screenshot 2026-11-02 124024" src="https://github.com/user-attachments/assets/7ba382e7-d88d-4e46-a076-ed7b3300af38" />
<img width="1207" height="708" alt="image" src="https://github.com/user-attachments/assets/718fb030-da86-437e-831a-0099c0071c0c" />
