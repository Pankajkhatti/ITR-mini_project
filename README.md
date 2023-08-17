# ITR-mini_project
# Task 1
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation


# Link lengths
l1 = 2.5
l2 = 2.5
trajectory = []
num_steps = 100
t_max = 10.0


num_points = int(input("Enter the number of points in the trajectory: "))


for i in range(num_points):
    x = float(input(f"Enter x-coordinate for point between 1 to 4{i+1}: "))
    y = float(input(f"Enter y-coordinate for point between 1 to 4 {i+1}: "))
    trajectory.append((x, y)) 

theta1i = []
theta2i = []

for x, y in trajectory:
    theta = np.arccos(((x ** 2) + (y ** 2) - (l1 ** 2) - (l2 ** 2)) / (2 * l1 * l2))
    q1 = np.arctan2(y, x) - np.arctan2(l2 * np.sin(theta), l1 + l2 * np.cos(theta))

    theta1 = q1
    theta2 = theta + q1
    theta1i.append(theta1)
    theta2i.append(theta2)

num_points = len(trajectory)

fig, ax = plt.subplots(figsize=(6, 6))
ax.set_xlim(-5, 5)
ax.set_ylim(-5, 5)

link1, = ax.plot([], [], 'o-', lw=2)
link2, = ax.plot([], [], 'g-', lw=2)


def init():
    link1.set_data([], [])
    link2.set_data([], [])
    return link1, link2


def animate(i):
    if i < num_points:
        theta1 = theta1i[i]
        theta2 = theta2i[i]

        x1 = l1 * np.cos(theta1)
        y1 = l1 * np.sin(theta1)
        x2 = x1 + l2 * np.cos(theta2)
        y2 = y1 + l2 * np.sin(theta2)

        link1.set_data([0, x1], [0, y1])
        link2.set_data([x1, x2], [y1, y2])

    return link1, link2

print(trajectory)

x_points = []
y_points = []

for tup in trajectory:
    x_points.append(tup[0])
    y_points.append(tup[1])




# Plot the original points and the interpolated curve
plt.scatter(x_points, y_points, color='red', label='Data Points')
plt.plot(x_points, y_points, label='Cubic Spline Curve')


# Create the animation
ani = FuncAnimation(fig, animate, frames=num_points, init_func=init, blit=True, interval=(t_max / num_steps)*1000)

# Set labels, title, and grid
plt.xlabel('x')
plt.ylabel('y')
plt.title('2R Manipulator: Trajectory Animation')
plt.grid()

# Display the animation
plt.show()

from IPython.display import HTML
HTML(ani.to_jshtml())
