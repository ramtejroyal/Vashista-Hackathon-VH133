# Vashista-Hackathon-VH133
VH 133
import time

class SmartHelmet:
    def __init__(self):
        self.temperature = 0
        self.humidity = 0
        self.gas_level = 0
        self.battery_level = 100
        self.location = (0, 0)
        self.alert_triggered = False

    def update_sensors(self):
        # Simulated sensor data update
        self.temperature = self.read_temperature()
        self.humidity = self.read_humidity()
        self.gas_level = self.read_gas_level()
        self.battery_level -= 1  # Simulate battery depletion

    def read_temperature(self):
        # Simulated temperature reading
        return 25  # Placeholder value

    def read_humidity(self):
        # Simulated humidity reading
        return 50  # Placeholder value

    def read_gas_level(self):
        # Simulated gas level reading
        return 0.1  # Placeholder value

    def send_data_to_server(self):
        # Simulated data transmission to server
        print("Sending sensor data to server...")

    def check_battery_level(self):
        if self.battery_level <= 10:
            self.trigger_alert("Low battery level!")

    def check_gas_level(self):
        if self.gas_level > 0.05:  # Placeholder threshold
            self.trigger_alert("High gas levels detected!")

    def trigger_alert(self, message):
        print("ALERT:", message)
        self.alert_triggered = True

    def update_location(self, new_location):
        self.location = new_location

    def display_data(self):
        print("Temperature:", self.temperature)
        print("Humidity:", self.humidity)
        print("Gas Level:", self.gas_level)
        print("Battery Level:", self.battery_level)
        print("Location:", self.location)

# Instantiate the SmartHelmet object
helmet = SmartHelmet()

# Main loop
while True:
    # Update sensor data
    helmet.update_sensors()

    # Check conditions
    helmet.check_battery_level()
    helmet.check_gas_level()

    # Send data to server
    helmet.send_data_to_server()

    # Display data on helmet display
    helmet.display_data()

    # Delay for simulation purposes
    time.sleep(1)
