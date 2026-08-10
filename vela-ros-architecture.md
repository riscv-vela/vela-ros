## Vela-ROS Package Composition

`vela-ros` packages the ROS 2 Jazzy stack for riscv64 out of the upstream source archive, then installs the resulting Debian packages onto the Vela platform.

```mermaid
flowchart TB
    SRC["RiscV ROS2 Jazzy source archive
packages.ros.org · apt-get source repo"]

    subgraph VELAROS["Vela-ROS"]
        BASE["ROS2 Base Package(Jazzy)"]
        SLAM["SLAM"]
        NAV2["Nav2"]
        subgraph TOOLS[" "]
            direction TB
            EXPLORE["Explore-lite"]
            COLCON["colcon build tool"]
        end
        DDS["rti-fastDDS"]
        DEBS["RiscV64 ros2 prebuilt *.deb packages"]

        BASE ~~~ SLAM
        BASE ~~~ NAV2
        BASE ~~~ TOOLS
        SLAM ~~~ DDS
        NAV2 ~~~ DDS
        TOOLS ~~~ DDS
        DDS ~~~ DEBS
    end

    ENV["Vela-OS(Ubuntu24.04 RiscV64)"]

    SRC --> BASE
    DEBS --> ENV

    style VELAROS fill:none,stroke:#000000,stroke-width:4px
    style TOOLS fill:none,stroke:none
    style DEBS fill:none,stroke:none
    linkStyle 7,8 stroke:#4472C4,stroke-width:1.2px
```
### Summary

1. `vela-ros` rebuilds the full ROS 2 Jazzy stack for riscv64 from the upstream `packages.ros.org` source archive via `apt-get source`.
2. The Vela-ROS build set spans core middleware (DDS), the ROS 2 base packages, and the navigation/mapping stack (Nav2, SLAM, Explore-lite).
3. `colcon` is the underlying build tool driving the per-package Debian packaging pipeline.
4. Every component is compiled into riscv64-native, prebuilt `*.deb` packages. 
5. The resulting packages are installed onto Vela-OS (Ubuntu 24.04, riscv64), producing a ready-to-run ROS 2 Jazzy robotics environment.
