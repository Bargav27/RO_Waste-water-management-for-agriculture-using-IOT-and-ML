Create a Mock Serial Class to simulate serial data

import serial
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
import threading
import time
import random

# Mock serial class to simulate data
class MockSerial:
    def __init__(self):
        self.data = ""
        self.lock = threading.Lock()

    def readline(self):
        with self.lock:
            line = self.data
        time.sleep(1)
        return line.encode('utf-8')

    def write_data(self, data):
        with self.lock:
            self.data = data

# Function to simulate the ESP32 sending data
def simulate_data(serial_port):
    while True:
        soil_moisture_value = random.randint(300, 700)
        data = f"Soil Moisture: {soil_moisture_value}\n"
        serial_port.write_data(data)
        time.sleep(2)

# Replace 'serial.Serial' with 'MockSerial' for testing
ser = MockSerial()

# Start a thread to simulate the ESP32 data
thread = threading.Thread(target=simulate_data, args=(ser,))
thread.daemon = True
thread.start()

x_data = []
y_data = []

def update(frame):
    line = ser.readline().decode('utf-8').strip()
    if "Soil Moisture:" in line:
        value = int(line.split(": ")[1])
        x_data.append(frame)
        y_data.append(value)
        if len(x_data) > 100:
            x_data.pop(0)
            y_data.pop(0)
        plt.cla()
        plt.plot(x_data, y_data)
        plt.xlabel('Time')
        plt.ylabel('Soil Moisture Value')
        plt.title('Live Soil Moisture Data')

ani = FuncAnimation(plt.gcf(), update, interval=1000)

plt.show()
