# ChatGPT Sample Code NEO-6M GPS Module

- [NEO-6M GPS Coordinates](https://chatgpt.com/share/678e56d7-ec68-8002-a3c1-4ac879d6a0ec)

```
Please provide sample circuitpythin code to read the gps coordinates of "Satellite Positioning NEO-6M"?
```

Below is a sample CircuitPython code to read GPS coordinates from a "Satellite Positioning NEO-6M" GPS module using the `adafruit_gps` library. Ensure that your GPS module is correctly connected to your board's UART pins.

### Requirements
1. Install the `adafruit_gps` library in your CircuitPython `lib` folder. You can download it from the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).
2. Connect the NEO-6M GPS module's TX pin to the board's RX pin, and the GPS module's RX pin to the board's TX pin.

### CircuitPython Code

```python
import time
import board
import busio
import adafruit_gps

# Create a UART connection for the GPS module
uart = busio.UART(board.TX, board.RX, baudrate=9600, timeout=10)

# Create a GPS module instance using the UART connection
gps = adafruit_gps.GPS(uart, debug=False)

# Initialize the GPS module
gps.send_command(b"PMTK314,0,1,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0")
gps.send_command(b"PMTK220,1000")  # Update rate: 1 Hz

last_print = time.monotonic()

while True:
    # Update GPS data
    gps.update()

    # Check if we should print updated GPS information
    current_time = time.monotonic()
    if current_time - last_print >= 1.0:  # Print every second
        last_print = current_time

        if not gps.has_fix:
            print("Waiting for fix...")
            continue

        # Print GPS coordinates
        print("=" * 40)
        print(f"Latitude: {gps.latitude:.6f}°")
        print(f"Longitude: {gps.longitude:.6f}°")
        print(f"Fix quality: {gps.fix_quality}")

        # Optional: Print other data if available
        if gps.satellites is not None:
            print(f"Satellites: {gps.satellites}")
        if gps.altitude_m is not None:
            print(f"Altitude: {gps.altitude_m} meters")
        if gps.speed_knots is not None:
            print(f"Speed: {gps.speed_knots} knots")
        if gps.track_angle_deg is not None:
            print(f"Track angle: {gps.track_angle_deg}°")
        if gps.horizontal_dilution is not None:
            print(f"Horizontal dilution: {gps.horizontal_dilution}")
        if gps.height_geoid is not None:
            print(f"Height geoid: {gps.height_geoid} meters")
```

### Explanation of Code
1. **UART Connection**: The NEO-6M GPS module communicates via UART, and you must set the correct `TX` and `RX` pins of your board.
2. **GPS Initialization**: The commands `PMTK314` and `PMTK220` configure the GPS module for data output and update rate.
3. **Reading GPS Data**: The `gps.update()` method fetches new GPS data, and the `gps.has_fix` property checks if the GPS has acquired a fix.
4. **Output**: The code prints latitude, longitude, and additional optional data (e.g., altitude, speed).

### Connecting the GPS Module
- **VIN**: Connect to 3.3V or 5V on your board.
- **GND**: Connect to GND on your board.
- **RX**: Connect to the TX pin on your board.
- **TX**: Connect to the RX pin on your board.

Let me know if you have further questions!
