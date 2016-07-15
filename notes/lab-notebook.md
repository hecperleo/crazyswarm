2016-07-14.2 - JAP
------------------
TODO list
- tune xy controller
- create "flash all" script
- create "reboot all" script
- more automated method to compute ctr of mass from marker config (worth it??)
- test ellipse trajectory
- test larger swarm
- compute layout for hovering 49 (42?)
- think about how we want to do real-time control
- think about how to do large-swarm position initialization
- increase object tracker robustness to outliers
- compute x-staggered positions to render text from a side view
- create "avoid human" planner/controller
- create "conductor wand" planner/controller

2016-07-14 - JAP
----------------
Hypothesized that latency in the object tracker might be introducing 
enough delay to cause oscillation.
Instrumented code and found that latency is under 1ms, not a problem.
Found that our object tracker was giving a translational offset of a few cm.
Manually fixed the offset, flight is now nearly as stable as Vicon tracker.
**Putting the object''s center of mass in the right location is really important.**

Matt suggested using Vicon''s tracking, with our role reduced to
detecting and fixing object identity misassignments.
Accuracy of new system is now good enough to track 3 nearly-identical objects robustly,
but if you copy the same object 3 times, it will assign them all to one crazyflie.
Seems like there should be a way to get around this, but we can''t find it.

Raw data from Vicon still contains much noise and spurious markers.
Filtering will be necessary in our tracker.

Fixed some firmware crash bugs:
- name of log variable was too long
- stack overflow after increasing computation in `trajectory_poly.c`
- assert does nothing unless using JTAG debugger

Added better takeoff/landing trajectories with linear yaw interpolation.
Fixed a bug where ctrl expected yaw setpoints in degrees but we were giving radians.
This led us to realize that our yaw gains in ctrl were way too low.
Tuning yaw gains gave us probably best hover yet.
Now XY translation gains need work.

Would like to get an adjustable-speed fan to test controller robustness.

2016-07-13 - JAP
----------------
Set up new Vicon PC.
Attempted to use object tracker but found quick tracking loss.
Corrected too-low Euler angle limits.
Now tracks well but still gives extremely unstable flight.

Flat-black Testors enamel marker is very good fix for reflective USB connector,
but it also scratches off easily

Attempted to run static analyzers:
- Clang complains about GCC-only directives
- Slint found some interesting issues, but only understands C89 syntax; rejects decl in for-loop
- cppcheck finds no issues

2016-07-12 - JAP
----------------
Tuned controller.
TODO: what else?

2016-07-11 - JAP
----------------
Refined camera cable management.
Identified source of strange blobs: aperture settings.
If aperture is too large, camera detects many spurious markers.
If too small, its maximum detection distance is reduced.
Optimal aperture seems to be 4.0 or maybe 5.6.

2016-07-10 - JAP
----------------
Set up new Vicon cameras.
All OK except one showing strange blobs and many spurious markers.

2016-07-08 - JAP
----------------
Set up trusses for new Vicon system.

2016-07-06 - JAP
----------------
Identified matte black polyimide (Kapton) tape as a possible solution for spurious markers on shiny parts.
Requested samples of the following products:
- 3M 7412B
- DuPont Kapton B
- Nitto UTS-30BAF
- Polyonics XT-719

2016-07-05 - JAP
----------------
We had a support call with Vicon regarding the old system:
- Got a temporary license for Tracker 3; it calibrates *much* faster than 2.2, with better results.
- Identified that our calibration wand is not straight
  - "Active" wand from new system gives ~5x reduction in image error
  - Feet on active wand do not have enough travel to successfully level the device on our floor
- Identified that our cameras are out of focus. Markers appear blurry with grey halos.
- Attempted to refocus cameras, but:
  - net blocks us from inside and is difficult to remove
  - wide ladder base + clutter make it impossible to get close to cameras from the outside
  - lenses are focus- and aperture-locked with tiny set screws
  - cables are bound to cage structure, making it difficult to take cameras down
    for easier manipulation while still observing image feed
- Failed to refocus any cameras successfully
- Noticed that shiny USB connector, battery, and possibly motors sometimes appear as spurious markers

2016-06-29 - JAP
----------------
- Polynomial timestretching fixed
- Github sub-repos done
- JTAG tether probably not possible - recommended length is like 8 inches
- Met w/ advisors, moving forward with temporary setup in unfinished new building
- Need to figure out a truss/support for Vicon in new space; tripods not so great

2016-06-25 - JAP
----------------
- Piecewise polynomial trajectories are working
- Need to fix polynomial timestretching (TODO)
- Need to tune controller better for position tracking
- Investigate making a JTAG + power tether??
- Organize code into github sub-repos (goal: one pull + make to set up) (TODO: research git features)