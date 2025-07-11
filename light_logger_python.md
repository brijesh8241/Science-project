# Science-project
# python code for Light Intensity Logger


import serial
import time
import matplotlib.pyplot as plt

# Setup serial communication (COM port may vary, e.g., "COM3" or "/dev/ttyUSB0")
ser = serial.Serial('COM3', 9600, timeout=1)
time.sleep(2)  # Give time for Arduino to reset

light_values = []
timestamps = []
start_time = time.time()

print("Logging started... Press Ctrl+C to stop.")

try:
    while True:
        line = ser.readline().decode('utf-8').strip()
        if line:
            light = int(line)
            current_time = time.time() - start_time
            light_values.append(light)
            timestamps.append(current_time)
            print(f"Time: {current_time:.1f}s | Light: {light}")
except KeyboardInterrupt:
    print("Logging stopped.")

# Plot results
plt.plot(timestamps, light_values, color='orange')
plt.title("Light Intensity Over Time")
plt.xlabel("Time (s)")
plt.ylabel("Light Value (0-1023)")
plt.grid(True)
plt.show()

# Cleanup
ser.close()
