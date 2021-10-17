# Hardware

## LiDAR

Light Detection And Ranging

Yields point cloud

Parameters:

- FOV (Field of View) (horizontal, vertical, ...)
- Angle resolution
- Spinning rate: e.g. 5-20Hz

Applications:

- Mapping?
- Perception
- Localization

Different types:

- Mechanical rotation (mostly used; easy to be broken, not reliable)
- MEMS-mirror: just rotate a small mirror (reliable)
- Optical phase array: doesn't move; use electricity to control the direction of laser; more challenging production (but more reliable)
- Flash LiDAR

Raw data:

- distance
- vertical or horizontal angle
- timestamp

Evolution

- Smaller & Cheaper
- Denser data
- Solid state

Advantages:

- Accurate depth (more accurate than radar)
- Active sensor
- Robust against ambient light

Disadvantages:

- Sparse (1080 of camera vs 64)
- Bulky (taking up much space; large and unwieldy)

## Camera

Applications:

- Obstacle detection / classification
- Traffic light (sometimes not easy)

Important specs:

- Dynamic range: cover different levels of brightness
- Global shutter vs rolling shutter (cheaper; can read the data line by line; need to be corrected)

Advantages:

- Low cost
- Rich data
- Color

Disadvantages:

- Only 2D info (need to reconstruct 3D scenes)
- Affected by ambient light
- Computation burden
- Storage burden
- Bandwidth burden

## Radar

Radio detection and ranging

Determine the range, angle and **velocity** of objects

Emits **chirps**?

short range mode or far range mode

Advantages:

- Usable under bad weather
- Good longitude speed accuracy

Disadvantages:

- Bad lateral speed accuracy
- False positives

## GPS

Real Time Differential GPS

- DGPS receiver
- DGPS site

## IMU

Inertial Measurement Unit

measures relative ego-position and ego-pose

drift over time

## Compute System

SoC: System on a chip