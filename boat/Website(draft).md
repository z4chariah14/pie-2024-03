# Principles of Integrated Engineering (PIE) Fall 2024 Project

## Homepage (High-Level Overview)

### **Project Title**
> *Autonomous GPS-Guided Boat Navigation System*

### **Team Members**
- **Belle See and Sage Gilbert-Diamond** - Electrical/Software Design
- **Filip Kypriotis, Jade Campbell and Zac Adeboye** - Mechanical Design
- **Everyone** - Testing and Integration

### **Project Summary**
This project focuses on building an autonomous boat navigation system using GPS and ESC motor controls. The boat calculates its heading and adjusts its path based on location data to navigate through predefined waypoints. 

### **Final Product Showcase**
![Final Product Image](Images\Boat.jpg) ![CAD of Final Boat](Images\Boat_whole.png)

[Watch the final product in action](path/to/video.mp4)

---

## Budget Breakdown

### **Overview**
Summary of how the $250 budget was allocated.

### **Table of Components**
| Component       | Description            | Link                                                                                                         | Unit Cost | Units | Cost    |
|-----------------|------------------------|-------------------------------------------------------------------------------------------------------------|-----------|-------|---------|
| Compass         | Compass Module         | [Amazon Link](https://www.amazon.com/dp/B0855TZV1J)                                                         | $7.99     | 1     | $7.99   |
| GPS             | GPS Module             | [Amazon Link](https://www.amazon.com/dp/B084MK8BS2)                                                         | $12.99    | 1     | $12.99  |
| Foam Blocks     | -                      | [Amazon Link](https://www.amazon.com/dp/B008HV11DE)                                                         | $39.99    | 1     | $39.99  |
| Thermal Paste   | -                      | [Amazon Link](https://www.amazon.com/dp/B0795DP124)                                                         | $6.99     | 1     | $6.99   |
| Super Glue      | -                      | [Amazon Link](https://www.amazon.com/dp/B00C32ME6G)                                                         | $12.59    | 1     | $12.59  |
| Motor           | VGEBY1 F2838-350KV     | [Amazon Link](https://www.amazon.com/dp/B08LN1MJ7X)                                                         | $26.69    | 2     | $53.38  |
| ESC             | 30A ESC                | [Amazon Link](https://www.amazon.com/dp/B071GRSFBD)                                                         | $16.99    | 2     | $33.98  |
| Battery         | -                      | [Amazon Link](https://www.amazon.com/dp/B08R72ZF6N)                                                         | $29.22    | 1     | $29.22  |
| Arduino         | Arduino Uno R3         | [Amazon Link](https://www.amazon.com/dp/B01EWOE0UU)                                                         | $16.99    | 1     | $16.99  |
| 20ft Cable      | USB-B to USB-C Cable   | [Amazon Link](https://www.amazon.com/dp/B0C2BNQVSF)                                                         | $8.96     | 1     | $8.96   |
| RC Controller   | RC Controller          | N/A (Provided by a teacher)                                                                                 | $59.90    | 1     | $0.00   |

**Total Cost: $223.08**

### **Key Notes**
- The compass was broken, so the project adapted by relying solely on GPS data.
- Components like foam and glue were used for the physical assembly.

---

## Data and Energy Flow

### **Data Flow Diagram**
![Data Flow Diagram](path/to/data_flow_diagram.jpg)

### **Energy Flow Diagram**
![Energy Flow Diagram](path/to/energy_flow_diagram.jpg)

### **Explanation**
The boat relies on GPS data for navigation and sends motor control signals via PWM to the ESCs. Data from the GPS is processed to calculate current and target headings.

---

## System Diagrams 

### **Circuit Design**
- **Description**: The circuit connects GPS and ESC components to the Arduino.
- **Connections**:
  - **ESCs**: Connected to **Digital Pins 9 and 10** for PWM motor control.
  - **GPS Module**: RXD connected to **Analog Pin 3**, TXD to **Analog Pin 4**.
- **Diagram**:
![Circuit Diagram](Images/ELECDESIGN.png)

---

## Evolution of the Code and Circuit Design

### **Circuit Design**
The circuit has always stayed the same since it’s a simple design where we just plug into established circuit elements such as an ESC and a GPS module.

### **Code Evolution**
1. **Initial Logic**:
   - Movement was determined by the boat’s position relative to the final destination.
   - If the boat moved past the destination on either latitude or longitude, it would switch direction (left/right).
   - *Issue*: This caused the boat to take inefficient routes.

2. **Second Iteration**:
   - Tracked previous and current coordinates to calculate if the boat was moving closer to the destination along latitude or longitude.
   - Flipped direction if the boat got further away.
   - *Issue*: The boat failed to account for situations where it faced the wrong direction entirely.

3. **Final Iteration**:
   - Calculated the current heading (based on previous and current coordinates) and compared it to the target heading (current to destination coordinates).
   - If the heading deviated beyond 45 degrees, the boat corrected its course.
   - Added logic to switch direction based on proximity to the target.

### **Final Code Design Elements**
- Initial delay to allow user to place the boat in water.
- Saves the start coordinates and calculates waypoints.
- Runs a navigation cycle every 4 seconds:
  1. Adjusts heading if it deviates beyond 45 degrees.
  2. Stops adjusting when close to the target coordinate.
- Switches waypoints within 10 meters and stops motors at the final destination for 30 seconds before returning to the start point.

### **Troubleshooting Errors**
1. **Unreliable GPS Connection**:
   - Solved by soldering wires to stabilize the GPS signal.

2. **Broken Compass**:
   - Adapted navigation logic to use GPS data only by calculating the boat’s heading from current and previous positions.

3. **Overflow Issues**:
   - GPS stopped sending updates when variables were initialized within the main loop.
   - Moved initialization outside the loop to resolve this.

4. **Drift**:
   - Used heading calculations between current and previous positions to detect drift and correct the course.

---

## Detailed Design Descriptions

### **Firmware Design**
#### **Dependencies**
- **SoftwareSerial**: Read data from the GPS module ([Docs](https://docs.arduino.cc/learn/built-in-libraries/software-serial/))
- **TinyGPS++**: For parsing GPS data ([Library](https://github.com/mikalhart/TinyGPSPlus))
- **PWMServo**: For motor control ([Library](https://reference.arduino.cc/reference/en/libraries/pwmservo/))

**Code Repository**: [GitHub Link](https://github.com/bellesea/boat/blob/main/sketch.ino)

---

# Mechanical Aspects 

## **Boat Hulls**
- **Design Concepts**:
  - Hull dimensions were determined using formulas from [Catamaran Hull Design Formulas](https://www.catamaransite.com/reference/catamaran_hull_design_formulas/).
  - MATLAB was used to compile these formulas, ensuring the calculated dimensions achieved proper buoyancy and stability.
   ![Boat Hulls](Images\Hull1.png) ![Hull](Images\Hull2.png) ![Hull_2](Images\Hull3.png)
- **Fabrication Process**:
  1. 1-inch foam layers were glued together with wood glue.
  2. The glued foam blocks were CNC-milled into shape based on the validated dimensions.
  3. Final finishing included sanding, coating with fiberglass and epoxy for waterproofing, and spray-painting for durability and aesthetics.
  ![Hull](Images\CNCHULL.png)

## **Thrust System**
- **Design Goals**:
  - Ensure motors deliver adequate propulsion for navigating currents and waypoints.
  - Protect motors from debris (e.g., seaweed and branches) while maintaining efficiency.
- **Implementation Details**:
  - CAD files from a BlueROV thruster were modified to fit the brushless motor dimensions.
  - Caps were added to both ends of the motor to prevent debris intake, which has effectively minimized issues during testing.
  - Motors were secured to the hulls using custom 3D-printed mounts, reinforced with epoxy and super glue for stability.
   ![Thruster](Images\Thruster_1.png) ![Thruster](Images\Thruster_2.png)

## **Waterproofing**
- **Hull Waterproofing**:
  - The foam hulls were coated with fiberglass and epoxy for structural integrity and waterproofing.
  - Spray paint provided an additional layer of protection against water infiltration.
- **Electrical Housing Waterproofing**:
  - The platform housing electrical components was spackled, sanded, and painted to create a water-resistant enclosure.
  - The lid design included tight tolerances to block water splashes while remaining removable for maintenance and upgrades.

## **Electrical Housing Platform**
- **Design Features**:
  - Inspired by catamaran architecture, the housing is elevated and arched, protecting critical electronics from water exposure.
  - Front and rear openings were added to enable water flow for ESC cooling.
  - Additional holes allow direct connections between motors and ESCs, ensuring clean and efficient wiring.
  ![Platform](Images\Platform_1.png) 

## **Motor Selection**
- **Considered Options**:
  - Motors specifically designed for underwater applications were evaluated but rejected due to extended delivery times.
  - Brushless motors were selected for their high torque-to-speed ratio, critical for consistent thrust in aquatic environments.
- **Selection Rationale**:
  - The VGEBY1 brushless motors demonstrated sufficient torque and durability during external testing, such as their ability to handle high-stress scenarios like drone crashes.

## **ESC Coolers**
- **Design and Fabrication**:
  - The ESC coolers were modeled after PC cooling systems, leveraging the boat’s motion to pass water through cooling channels.
  - Aluminum stock was machined into radiator-like channels, which were mounted to the ESCs using thermal paste and glue.
- **Passive Cooling Operation**:
  - As the boat moves, water is channeled through tubing into the ESC cooler, dissipating heat without additional power requirements.
  ![ESC Coolers](Images\ESC_cooler1.png) ![ESC Coolers](Images\ESC_cooler2.png)

---

## **Subsystem Relationships**
The mechanical, electrical, and software components work together to achieve the boat's autonomous functionality:

1. **Motor Mounts and ESCs**:
   - Motors are securely mounted to the hulls using 3D-printed brackets that are reinforced with epoxy and super glue.
   - Each motor is powered by an ESC, which receives PWM signals from the Arduino to control speed and direction.

2. **Electrical Housing Platform**:
   - The housing protects electrical components like the Arduino and ESCs while allowing for connections to motors and sensors.
   - ESC coolers, mounted on the platform, rely on boat speed to pass water through aluminum channels for passive cooling.

3. **Boat Hulls and Waterproofing**:
   - The hulls support the motor mounts and electrical housing while ensuring buoyancy.
   - Fiberglass, epoxy, and spray paint provide waterproofing, maintaining structural integrity.

4. **Software Integration**:
   - The Arduino reads GPS data to calculate heading and target waypoints.
   - Software logic adjusts PWM signals sent to the ESCs, directing the motors to correct the boat's course.
   - Feedback from the GPS ensures that the boat continuously adjusts to stay on track.

---

## Photos and Videos

### **Gallery**
#### Photos

![Platform](Images\plat.png) ![Boat](Images\Boat(comp).jpg) ![Electrical testing](Images\elec.jpg) ![ESC Cooler](Images\ESC_cooler.jpg) ![Platform](Images\Platform(manufacture).png)

#### Videos
[Watch Motor Testing](videos\IMG_0944.mov)

[CNC Milling Hull](videos\IMG_3760.MOV)

---

## Reflection and Documentation

### **Lessons Learned**
- Dealing with broken hardware (compass) forced us to adapt navigation using GPS data only.
- Debugging overflow issues in the main loop ensured GPS reliability.
- ADD MORE LATER

### **Tradeoffs**
- Without a working compass, navigation logic relied on heading calculations derived from GPS coordinates.
- MORE!!!!!!!! LATER

### **Acknowledgments**
- Collaborators: *Blank for now*
- Libraries and external resources: TinyGPS++, PWMServo, SoftwareSerial.