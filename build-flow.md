## Relationship Between `vela-ros` and RISC-V-adapted ROS Packages

The following repositories provide RISC-V-adapted package sources that are consumed and built by `vela-ros`.

```mermaid
flowchart TB
    subgraph SOURCES["RISC-V-adapted ROS 2 Package Sources"]
        CATKIN["python3-catkin-pkg-modules<br/>ROS package metadata tools"]
        click CATKIN href "https://github.com/riscv-vela/python3-catkin-pkg-modules" "ROS package metadata tools repository"
        MIMICK["ros-jazzy-mimick-vendor<br/>Mocking library vendor package"]
        click MIMICK href "https://github.com/riscv-vela/ros-jazzy-mimick-vendor" "Mocking library vendor package repository"
        BACKWARD["backward_ros<br/>C++ stack trace support"]
        click BACKWARD href "https://github.com/riscv-vela/backward_ros" "C++ stack trace support repository"
        OGRE["ros-jazzy-gz-ogre-next-vendor<br/>Gazebo rendering dependency"]
        click OGRE href "https://github.com/riscv-vela/ros-jazzy-gz-ogre-next-vendor" "Gazebo rendering dependency repository"
        MPPI["ros-jazzy-nav2-mppi-controller<br/>Nav2 MPPI controller plugin"]
        click MPPI href "https://github.com/riscv-vela/ros-jazzy-nav2-mppi-controller" "MPPI controller plugin repository"
    end

    subgraph BINARIES["RISC-V-adapted ROS 2 Package Binaries"]
		RTI_CONNEXT_DDS["rti-connext-dds"]
	end

    VELA["vela-ros<br/>ROS 2 Jazzy Build and Packaging<br/>Orchestrator for RISC-V64"]

    DEBS[/"Build Artifacts<br/>RISC-V64 Debian Packages<br/>ros-jazzy-*.deb · python3-*.deb"/]

    ENV(["Vela Platform<br/>with ROS 2 Jazzy on RISC-V64"])
    click ENV href "https://github.com/riscv-vela/vela" "Vela repository"

    CATKIN -->|"package source"| VELA
    MIMICK -->|"package source"| VELA
    BACKWARD -->|"package source"| VELA
    OGRE -->|"package source"| VELA
    MPPI -->|"package source"| VELA

    RTI_CONNEXT_DDS -->|"package binary"| VELA

    VELA -->|"builds with dpkg-buildpackage"| DEBS
    DEBS -->|"installs with apt"| ENV
```

### Relationship

- The five repositories are not Git submodules of `vela-ros`.
- They provide customized or RISC-V64-adapted package sources.
- `vela-ros` clones and builds these sources when required.
- The direct build outputs are RISC-V64 Debian package files.
- Installing the generated packages creates the Vela ROS 2 Jazzy runtime environment.
