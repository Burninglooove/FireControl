### 这是一款基于Valkyrie skies 2、CBC、CC:Tweaked、CCVS的火控计算机
### This is a fire control computer based on Valkyrie skies 2, CBC, CC:Tweaked, CCVS

火控需要玄学Mod作为API （0.0.4+）：
Fire control requires Metaphysics Mod(0.0.4+) as API：
> @kallen https://github.com/KallenKas024/Metaphysics/tree/main

该火控需要依赖以下mod：
This fire control also requires the following mods:

* Valkyrien Skies 2
* Create
* Create Big Cannons
* CC:Tweaked
* CC:VS
* Tom's Peripherals
* Metaphysics
* VS Addition

以下mod非强制，但推荐安装
The following mods are not mandatory, but recommended to install
* clockwork
* vMod
* Some Peripherals
* Create: Interactive
* UnlimitedPeripheralWorks(推荐，提供外设代理-无线外设) (reason: peripheral proxy)
* Some Peripherals (火控的头瞄模式2支持Raycast眼镜) (Fire control head aiming mode 2 supports Raycast goggles)

## MWVS radar console mode

This branch adds a simplified control-center mode for
`mianbaos-modernwarfare-vs2-compat`.

The ControlCenter GUI is kept as the radar display and target selection
surface, but its runtime logic is changed from CBC cannon-group fire control to
Modern Warfare missile radar control:

* The script looks for a CC:Tweaked peripheral named `mwvs_radar`.
* Ship scanning uses `mwvs_radar.getTargets(range)` first, with the old CCVS
  coordinate scan kept only as a fallback.
* Manual target selection and auto-select call `mwvs_radar.lock(shipId)`.
* Clearing the selected target calls `mwvs_radar.clearLock()`.
* CBC/rednet cannon dispatch loops are no longer started by this ControlCenter.

With this model, one Modern Warfare radar represents one missile group target.
Multiple Modern Warfare launchers can share that radar by using the same radar
channel. Newly launched missiles on that channel follow the radar's current
locked VS2 ship target, while missiles that have already received their
`shipId` keep tracking their assigned target.
