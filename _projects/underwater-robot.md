---
layout: page
title: Bio-Inspired Underwater Robot (GUJCOST Robofest 2.0)
description: A cuttlefish-inspired underwater robot using undulating fin propulsion, built for GUJCOST Robofest 2.0
img: assets/img/underwater-robot-1.jpg
importance: 2
category: work
pdf: underwater_robot_proposal.pdf
---

Built for **GUJCOST Robofest 2.0**, a state-level robot-making competition under Gujarat's Science, Technology and Innovation (STI) fund, this project set out to design and build an underwater robot inspired by one of nature's most efficient swimmers: the cuttlefish.

Team: Himanshu Laddhad, Atul Dhamija, Naman Jain, Dhruv Oza, and myself — mentored by Dr. Harshit K. Dave, SVNIT Surat.

## The Core Idea: Undulating Fin Propulsion

Rather than using a traditional propeller, the robot mimics the way a cuttlefish moves — using a long, wave-like fin that runs along the body. In our design, the main shaft is built from small links fixed inside slotted rods, connected to a flexible fin on each side. By driving the fin into a continuous undulating wave motion, the robot pushes water backward to move forward — and can turn by changing the direction of rotation on one side relative to the other.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/underwater-robot-2.jpg" title="Fin assembly render" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/underwater-robot-1.jpg" title="CAD drawing" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the undulating fin mechanism design. Right: CAD drawings of individual components (motion bar, end caps, fin holder, main frame).
</div>

## Buoyancy Control

To move up and down, the robot uses the same principle real submarines use: balancing buoyant force against gravity. Water-filled syringes act as ballast — a linear actuator pushes or releases water from the syringes, changing the robot's overall weight and adjusting its depth.

## Electronics & Control

- **Arduino Nano** as the main controller
- **FlySky RC transmitter/receiver**, communicating over the iBus protocol, for wireless control
- **L298N motor drivers** to run the fin motors and buoyancy actuator
- **Raspberry Pi + camera module** for onboard vision
- **IMU** for orientation sensing
- Custom PCB for wiring integration

Firmware was written in Arduino C++. One sketch reads the RC receiver's channels over iBus and maps stick input directly to servo position:

```cpp
int readChannel(byte channelInput, int minLimit, int maxLimit, int defaultValue) {
  uint16_t ch = ibus.readChannel(channelInput);
  if (ch < 100) return defaultValue;
  return map(ch, 1000, 2000, minLimit, maxLimit);
}
```

Another sketch drives multiple servos in a synchronized sweep to test and tune the fin's undulating motion before final assembly:

```cpp
void loop() {
  for (i = 1; i <= 180; i++) {
    servo1.write(i);
    servo2.write(i);
    servo3.write(i);
    servo4.write(i);
    delay(2);
  }
  for (i = 180; i > 0; i--) {
    servo1.write(i);
    servo2.write(i);
    servo3.write(i);
    servo4.write(i);
    delay(2);
  }
}
```

## Build Process

- Chassis and structural parts were fully **3D printed in PLA**, chosen for its availability and biocompatibility
- Design and analysis done in **SolidWorks**, with **ANSYS** for structural analysis
- Waterproofing achieved via the **oil-filled servo method** — filling servos with light-viscosity mineral oil (a technique borrowed from underwater RC hobbyist communities), combined with superglue and O-ring seals at joints
- Powered by an **Orange 5200mAh 3S LiPo battery** (11.1V, 40C/80C discharge rating)

## Real-World Applications

Beyond the competition, the design has genuine applications: studying aquatic life and habitats, low-cost underwater exploration, ship hull inspection (when paired with AI/vision), and environmental sampling like microplastic surveys.

## What's Next

Future versions are planned to be **amphibious** (walking on land as well as swimming) and to support **tethered operation** via a floating antenna for greater depth range.