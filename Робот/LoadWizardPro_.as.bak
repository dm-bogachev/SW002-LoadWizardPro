.AUXDATA
N_OX33    "grip.open[1]|Open gripper"
N_OX34    "grip.close[1]|Close gripper"
N_OX35    "grip.open[2]|"
N_OX36    "grip.close[2]|Close gripper"
N_OX37    "air.blow.on|Air blow on"
N_OX38    "air.blow.off|Air blow off"
N_OX78    "o.debug|Robot is in DEBUG mode"
N_OX84    "eo.grip.error[1]|Gripper open error"
N_OX92    "eo.grip.error[2]|Gripper open error"
N_OX115    "eo.process.err|Error in process"
N_OX129    "eo.shelf.opened[1]|Shelf is opened"
N_OX130    "eo.shelf.opened[2]|Shelf is opened"
N_OX131    "eo.shelf.opened[3]|Shelf is opened"
N_OX132    "eo.shelf.opened[4]|Shelf is opened"
N_OX133    "eo.shelf.unlock[1]|Request shelf unlock"
N_OX134    "eo.shelf.unlock[2]|Request shelf unlock"
N_OX135    "eo.shelf.unlock[3]|Request shelf unlock"
N_OX136    "eo.shelf.unlock[4]|Request shelf unlock"
N_WX33    "grip.opened[1]|Gripper opened"
N_WX34    "grip.opened[2]|Gripper opened"
N_WX35    "grip.sensor[1]|Gripper force applied"
N_WX36    "grip.sensor[2]|Gripper force applied"
N_WX84    "ei.check.grip[1]|Check gripper sensor"
N_WX92    "ei.check.grip[2]|Check gripper sensor"
N_WX133    "ei.shelf.state[1]|Shelf state is unlocked"
N_WX134    "ei.shelf.state[2]|Shelf state is unlocked"
N_WX135    "ei.shelf.state[3]|Shelf state is unlocked"
N_WX136    "ei.shelf.state[4]|Shelf state is unlocked"
N_WX137    "ei.shelf.failed|Fail to unlock shelf"
N_WX145    "ei.robot.speed[0]|Robot speed from PLC"
N_WX321    "d.wp.l[0,0]|Workpiece lengths"
N_WX329    "d.wp.l[1,0]|Workpiece lengths"
N_WX337    "d.wp.l[2,0]|Workpiece lengths"
N_WX345    "d.grip.jaw.full[1,0]|Gripper jaws full length"
N_WX353    "d.grip.jaw.body[1,0]|Gripper jaws body length"
N_WX361    "d.grip.jaw.full[2,0]|"
N_WX369    "d.grip.jaw.body[2,0]|Gripper jaws body length"
N_WX377    "d.cnc.jaws.full[1,0]|CNC jaws full length"
N_WX385    "d.cnc.jaws.body[1,0]|CNC jaws body length"
N_WX393    "d.cnc.jaws.full[2,0]|CNC jaws full length"
N_WX401    "d.cnc.jaws.body[2,0]|CNC jaws body length"
N_WX409    "d.plt.rows[0]|Number of rows on plate"
N_WX413    "d.plt.cell.odd[0]|Number of cells in odd row on plate"
N_WX417    "d.plt.cell.even[0]|Number of cells in even row on plate"
N_WX433    "d.plt.dx[0]|Distance between rows"
N_WX449    "d.plt.dy[0]|Distance between cells"
N_WX465    "d.plt.even.dy[0]|Extrs shift on even rows"
N_WX481    "d.plt.ox[0]|Distance between zero point and closest cell by X"
N_WX497    "d.plt.oy[0]|Distance between zero point and closest cell by Y"
N_INT10    "s.shelf.failed|Internal signal for shelf fail error"
N_INT102    "s.hmi.tool[1]|Selected tool on HMI"
N_INT103    "s.hmi.tool[2]|Selected tool on HMI"
N_INT104    "s.cnc.chuck[1]|Selected CNC chuck on HMI"
N_INT105    "s.cnc.chuck[2]|Selected CNC chuck on HMI"
N_INT106    "s.pr.tch.plate|Prime teach plate"
N_INT107    "s.pr.tst.plate|Prime test plate"
N_INT108    "s.pr.tch.shelf|Prime teach shelf"
N_INT109    "s.pr.tst.shelf|Prime test shelf"
.END
.INTER_PANEL_D
0,9,1,6,15
7,9,2,6,15
14,9,3,6,15
21,9,8,6,15
28,4,2,"TEACH TOOL","TOOL 1","TOOL 2","",10,4,4,2102,2103,0
29,8,"hmi.shelf.no"," Shelf No","",10,6,2,1,0
30,10,"","PCEXECUTE ","AUTOSTART","",10,4,6,1,"PCEXECUTE autostart.pc",0
31,8,"hmi.wp.id","   WP id","",10,8,2,1,0
35,2,"  PRIME","  TEACH","  PLATE","",10,4,3,2106,0
36,2,"  PRIME","  TEST","  PLATE","",10,4,3,2107,0
42,2,"  PRIME","  TEACH","  SHELF","",10,4,3,2108,0
43,2,"  PRIME","  TEST","  SHELF","",10,4,3,2109,0
.END
.INTER_PANEL_TITLE
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
"",0
.END
.INTER_PANEL_COLOR_D
182,3,224,244,28,159,252,255,251,255,0,31,2,241,52,255,
.END
.PROGRAM a.home ()
  ;
  ; Set small speed
  SPEED 100 MM/S ALWAYS
  ACCURACY 0.01 ALWAYS
  ; Move to home and home standby pos
  HOME
  HOME2  
  ;
.END
.PROGRAM a.main ()
  ;
  CALL log ("Main program executed")
  ;CALL safe.home
  ;
  WHILE TRUE DO
    .$pg.string = "state" + $ENCODE (/L, state)
    IF EXISTPGM (.$pg.string) THEN
      SCALL .$pg.string
    ELSE
      CALL log ("Error! Program is in wrong state. Reset state to 0")
      RETURN
    END
  END
  ;
.END
.PROGRAM autostart.pc ()
  ;
  CALL initialize.pc
  ;
  WHILE TRUE DO
    CALL check.speed.pc
    CALL check.teach.pc
    CALL check.limits.pc
  END
  ;
.END
.PROGRAM check.limits.pc ()
  ;
  hmi.shelf.no = MINVAL(hmi.shelf.no, 4)
  hmi.shelf.no = MAXVAL(hmi.shelf.no, 1)
  hmi.wp.id = MAXVAL(hmi.wp.id, 1)
  hmi.wp.id = MAXVAL(hmi.wp.id, 255)
  ;
.END
.PROGRAM check.speed.pc ()
  ;
  IF SIG (o.debug) THEN
    RETURN
  END
  ; Set speed from HMI at any time
  .speed = BITS (ei.robot.speed[0], 16)
  .speed = MAXVAL (.speed, 1)
  .speed = MINVAL (.speed, 100)
  IF .speed <> MSPEED THEN
    MON_SPEED .speed
    CALL log.pc1 ("Speed" + $ENCODE (.speed) + " was applied")
  END
  ;
.END
.PROGRAM check.teach.pc ()
  ;
  IF NOT SWITCH (REPEAT ) THEN
    IF SIG(s.pr.tch.plate) THEN
      MC PRIME plate.teach
    END
    ;
    IF SIG(s.pr.tst.plate) THEN
      MC PRIME plate.test
    END
    ;
    IF SIG(s.pr.tch.shelf) THEN
      MC PRIME shelf.teach
    END
    ;
    IF SIG(s.pr.tst.shelf) THEN
      MC PRIME shelf.test
    END
  END
  ;
.END
.PROGRAM disp.info.pc ()
  ;
  .$robot.name = $SYSDATA(ZROB.NAME)
  .robot.sn = SYSDATA (ZROB.MGFNO)
  .cont.sn = SYSDATA (CONT.NO)
  .$robot.str = "Robot: " + .$robot.name +" S/N: C" + $ENCODE (/L, .robot.sn)
  .$cont.str = "Controller: F60 S/N: C" + $ENCODE (/L, .cont.sn)
  IFPWPRINT 8, 1, 1, 5, 10 = .$robot.str, .$cont.str, " ", "Powered by Robowizard Co.Ltd."
  ;
.END
.PROGRAM grip.close (.grip.no,.time,.reverse)
  ;
  IF NOT .reverse THEN
    SIGNAL grip.close[.grip.no], -grip.open[.grip.no]
  ELSE
    SIGNAL grip.open[.grip.no], -grip.close[.grip.no]
  END
  TWAIT .time
  ;
  CALL log("Command close gripper" + $ENCODE(.grip.no))
  ;
.END
.PROGRAM grip.open (.grip.no,.time,.reverse)
  ;
  IF SIG(ei.check.grip[.grip.no]) AND SIG(grip.opened[.grip.no]) THEN
    CALL log("Gripper" + $ENCODE(.grip.no) + "already opened")
    RETURN
  END
  IF NOT .reverse THEN
    SIGNAL grip.open[.grip.no], -grip.close[.grip.no]
  ELSE
    SIGNAL grip.close[.grip.no], -grip.open[.grip.no]
  END
  TWAIT .time
  ;
  CALL log("Command open gripper" + $ENCODE(.grip.no))
  ;
.END
.PROGRAM initialize.pc ()
  ;
  CALL set.switches.pc
  CALL log.init
  CALL set.io.pc
  CALL set.vars.pc
  CALL disp.info.pc
  ;
  MC PRIME a.main
  TWAIT 0.5
  ;
  CALL log.pc1 ("Robot boot initialize completed")
  ;
.END
.PROGRAM log (.$msg)
  ;
  IF LEN (.$msg) > 55 THEN
    .$msg = $LEFT (.$msg, 55)
  END
  ;
  FOR .i = 1 TO log.max.count - 1
    $log.entry[.i] = $log.entry[.i + 1]
  END
  $log.entry[log.max.count] = $TIME + " " + .$msg
  ;
  FOR .i = 0 TO 11
    .$tmp[.i] = $log.entry[log.max.count - (11 - .i)]
  END
  ;
  IFPWPRINT 1, 1, 1, 9, 10 = .$tmp[0], .$tmp[1], .$tmp[2],  .$tmp[3]
  IFPWPRINT 2, 1, 1, 9, 10 = .$tmp[4], .$tmp[5], .$tmp[6],  .$tmp[7]
  IFPWPRINT 3, 1, 1, 9, 10 = .$tmp[8], .$tmp[9], .$tmp[10], .$tmp[11]
  ;
.END
.PROGRAM log.clear ()
  ;
  FOR .i = 1 TO log.max.count
    $log.entry[.i] = ""
  END
  ;
.END
.PROGRAM log.init ()
  ;
  ; Change this number if needed
  log.max.count = 256
  ;
  .$tmp1 = $ENCODE(/L, log.max.count)
  .$tmp2 = "$log.entry[" + .$tmp1 + "]"
  IF NOT EXISTCHAR (.$tmp2) THEN
    FOR .i = 1 TO log.max.count
      $log.entry[.i] = ""
    END
  END
  ;
.END
.PROGRAM log.pc1 (.$msg)
  ;
  IF LEN (.$msg) > 55 THEN
    .$msg = $LEFT (.$msg, 55)
  END
  ;
  FOR .i = 1 TO log.max.count - 1
    $log.entry[.i] = $log.entry[.i + 1]
  END
  $log.entry[log.max.count] = $TIME + " " + .$msg
  ;
  FOR .i = 0 TO 11
    .$tmp[.i] = $log.entry[log.max.count - (11 - .i)]
  END
  ;
  IFPWPRINT 1, 1, 1, 9, 10 = .$tmp[0], .$tmp[1], .$tmp[2],  .$tmp[3]
  IFPWPRINT 2, 1, 1, 9, 10 = .$tmp[4], .$tmp[5], .$tmp[6],  .$tmp[7]
  IFPWPRINT 3, 1, 1, 9, 10 = .$tmp[8], .$tmp[9], .$tmp[10], .$tmp[11]
  ;
.END
.PROGRAM pg0 ()
  ;
  state = 0
  CALL a.main
  ;
.END
.PROGRAM plate.teach ()
  ;
  ; Preparations for teaching
  ; Set tool
  IF SIG (s.hmi.tool[1]) THEN
    .tool.no = 1
  ELSE
    .tool.no = 2
  END
  TOOL tool.calib[.tool.no]
  ;
  JMOVE #wp.safe[.tool.no]
  ;
  LAPPRO #plate.pt.o[hmi.shelf.no, .tool.no], 20
  BREAK
  ; Teach this point for origin
  LMOVE #plate.pt.o[hmi.shelf.no, .tool.no] ; **== TEACH POINT ==**
  LAPPRO #plate.pt.o[hmi.shelf.no, .tool.no], 20
  ;
  LAPPRO #plate.pt.x[hmi.shelf.no, .tool.no], 20
  BREAK
  ; Teach this point for x direction
  LMOVE #plate.pt.x[hmi.shelf.no, .tool.no] ; **== TEACH POINT ==**
  LAPPRO #plate.pt.x[hmi.shelf.no, .tool.no], 20
  ;
  BREAK
  ; Teach this point for y direction
  LAPPRO #plate.pt.y[hmi.shelf.no, .tool.no], 20
  LMOVE #plate.pt.y[hmi.shelf.no, .tool.no] ; **== TEACH POINT ==**
  LAPPRO #plate.pt.y[hmi.shelf.no, .tool.no], 20
  ;
  ; Calculation
  POINT .po = #plate.pt.o[hmi.shelf.no, .tool.no]
  POINT .px = #plate.pt.x[hmi.shelf.no, .tool.no]
  POINT .py = #plate.pt.y[hmi.shelf.no, .tool.no]
  POINT .f = FRAME (.po, .px, .py, .py)
  POINT .f = .f + TRANS (-80, 0, 0)
  POINT shelf.frame[hmi.shelf.no, .tool.no] = .f
  ;
  JMOVE #wp.safe[.tool.no]
  ;
.END
.PROGRAM plate.test ()
  ;
  ; Preparations for testing
  ; Set tool
  IF SIG (s.hmi.tool[1]) THEN
    .tool.no = 1
  ELSE
    .tool.no = 2
  END
  TOOL tool.calib[.tool.no]
  ;
  JMOVE #wp.safe[.tool.no]
  ;
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(80, 0, 0), 20
  LMOVE shelf.frame[hmi.shelf.no, .tool.no] + TRANS(80, 0, 0)
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(80, 0, 0), 20
  ;
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(390+80, 0, 0), 20
  LMOVE shelf.frame[hmi.shelf.no, .tool.no] + TRANS(390+80, 0, 0)
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(390+80, 0, 0), 20
  ;
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(80, -630, 0), 20
  LMOVE shelf.frame[hmi.shelf.no, .tool.no] + TRANS(80, -630, 0)
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(80, -630, 0), 20
  ;
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(390+80, -630, 0), 20
  LMOVE shelf.frame[hmi.shelf.no, .tool.no] + TRANS(390+80, -630, 0)
  LAPPRO shelf.frame[hmi.shelf.no, .tool.no] + TRANS(390+80, -630, 0), 20
  ;
  JMOVE #wp.safe[.tool.no]
  ;
.END
.PROGRAM set.io.pc ()
  ;
  ; Gripper IO
  ;
  ; Gripper
  grip.open[1]          = 33
  grip.close[1]         = 34
  grip.open[2]          = 35
  grip.close[2]         = 36
  air.blow.on           = 37
  air.blow.off          = 38
  ; Gripper input
  grip.opened[1]        = 1033
  grip.opened[2]        = 1034
  grip.sensor[1]        = 1035
  grip.sensor[2]        = 1036
  ;
  o.debug               = 78
  ;
  eo.grip.error[1]      = 84
  eo.grip.error[2]      = 92
  eo.process.err        = 115
  ;
  eo.shelf.opened[1]    = 129
  eo.shelf.opened[2]    = 130
  eo.shelf.opened[3]    = 131
  eo.shelf.opened[4]    = 132
  ;
  eo.shelf.unlock[1]    = 133
  eo.shelf.unlock[2]    = 134
  eo.shelf.unlock[3]    = 135
  eo.shelf.unlock[4]    = 136
  ;
  ei.check.grip[1]      = 1084
  ;
  ei.check.grip[2]      = 1092
  ;
  ei.shelf.state[1]     = 1133
  ei.shelf.state[2]     = 1134
  ei.shelf.state[3]     = 1135
  ei.shelf.state[4]     = 1136
  ei.shelf.failed       = 1137
  ;
  ei.robot.speed[0]     = 1145 ; 8 bit
  ;
  ; Task data
  ; 0 - RAW; 1 - MID ; 2 - READY
  d.wp.l[0, 0]          = 1321 ; 8 bit
  d.wp.l[1, 0]          = 1329 ; 8 bit
  d.wp.l[2, 0]          = 1337 ; 8 bit
  d.grip.jaw.full[1, 0] = 1345 ; 8 bit
  d.grip.jaw.body[1, 0] = 1353 ; 8 bit
  d.grip.jaw.full[2, 0] = 1361 ; 8 bit
  d.grip.jaw.body[2, 0] = 1369 ; 8 bit
  d.cnc.jaws.full[1, 0] = 1377 ; 8 bit
  d.cnc.jaws.body[1, 0] = 1385 ; 8 bit
  d.cnc.jaws.full[2, 0] = 1393 ; 8 bit
  d.cnc.jaws.body[2, 0] = 1401 ; 8 bit
  ;
  d.plt.rows[0]         = 1409 ; 4 bit
  d.plt.cell.odd[0]     = 1413 ; 4 bit
  d.plt.cell.even[0]    = 1417 ; 4 bit
  d.plt.dx[0]           = 1433 ; 16 bit
  d.plt.dy[0]           = 1449 ; 16 bit
  d.plt.even.dy[0]      = 1465 ; 16 bit
  d.plt.ox[0]           = 1481 ; 16 bit
  d.plt.oy[0]           = 1497 ; 16 bit
  ;
  d.cnc.pg.no[0]        = 1513;;;
  d.chg.pg.no[0]        = 1521;;;
  d.air.pg.no[1, 0]     = 1529;;;
  d.air.pg.no[2, 0]     = 1537;;;
  d.wp.count[0]         = 1545;;;
  ;
  d.gp.reverse[1]       = 1561;;;
  d.gp.reverse[2]       = 1562;;;
  d.cnc.run.pg          = 1563;;;
  d.air.blow[1]         = 1564;;;
  d.air.blow[2]         = 1565;;;
  d.rdy.pick[1]         = 1566;;;
  d.rdy.pick[2]         = 1567;;;
  d.cnc.first           = 1568;;;
  d.gp.first            = 1569;;;
  ;
  d.i.change            = 1570;;;
  d.e.change            = 1571;;;
  ;
  d.gp.chg.i[1]         = 1577;;;
  d.gp.chg.i[2]         = 1578;;;
  ;
  d.gp.chg.e[1, 1]      = 1579;;;
  d.gp.chg.e[1, 2]      = 1580;;;
  d.gp.chg.e[2, 1]      = 1581;;;
  d.gp.chg.e[2, 2]      = 1582;;;
  ;
  s.shelf.failed        = 2010
  ;
  s.hmi.tool[1]         = 2102
  s.hmi.tool[2]         = 2103
  s.cnc.chuck[1]        = 2104
  s.cnc.chuck[2]        = 2105
  ;
  s.pr.tch.plate        = 2106
  s.pr.tst.plate        = 2107
  s.pr.tch.shelf        = 2108
  s.pr.tst.shelf        = 2109
  ;
.END
.PROGRAM set.switches.pc ()
  ;
  ; System switches
  CP ON
  PREFETCH.SIGINS OFF
  QTOOL OFF
  REP_ONCE ON
  HOLD.STEP ON
  DISP.EXESTEP ON
  PROG.DATE ON
  ABS.SPEED ON
  ERRSTART.PC ON  ;
  ;
.END
.PROGRAM set.tool (.tool.no)
  ;
  IF .tool.no <> 3 THEN
    TOOL tool.gripper[.tool.no]
  ELSE
    TOOL tool.pin
  END
  ;
  WEIGHT 20, 0, 0, 100, 0.1, 0.1, 0.1
  current.tool = .tool.no
  ;
  CALL log ("Tool" + $ENCODE (/L, .tool.no)+ " set")
  ;
.END
.PROGRAM set.vars.pc ()
  ;
  POINT tool.gripper[1] = TRANS (-103.3, 0, 104, -180, 90, 180)
  POINT tool.gripper[2] = TRANS (103.3, 0, 104, 0, 90, 180)
  POINT tool.calib[1] = TRANS (-202.8, 0, 104, -180, 90, 180)
  POINT tool.calib[2] = TRANS (202.8, 0, 104, 0, 90, 180)
  POINT tool.pin = TRANS (0, 112, 104, 90, 90, 0)
  ;
  hmi.shelf.no = 1
  hmi.wp.id = 1
  ;
.END
.PROGRAM shelf.close (.shelf.no)
  ;
  CALL log ("Closing shelf" + $ENCODE (.shelf.no))
  ;
  SPEED 60 ALWAYS
  ACCURACY 5 ALWAYS
  CALL set.tool (3)
  ; gripper.no, time, reverse
  CALL grip.close (1, 0, FALSE)
  CALL grip.close (2, 0, FALSE)
  ;
  POINT .start = shelf.close[.shelf.no, 1]
  POINT .end = shelf.close[.shelf.no, 2]
  ;
  JMOVE #shelf.safe
  BREAK
  $safe.flag = "shelf.safe"
  ;
  LMOVE .start + TRANS (150, 0, -50)
  LMOVE .start + TRANS (-50, 0, -50)
  $safe.flag = "shelf.work"
  LMOVE .start + TRANS (-50, 0, 0)
  SPEED 50 MM/S
  ACCURACY 0.1
  LMOVE .start
  BREAK
  ; Open shelf
  CALL log ("Request unlock shelf" + $ENCODE (.shelf.no))
  SIGNAL eo.shelf.unlock[.shelf.no]
  WAIT BITS (ei.shelf.state[1], 5) <> 0
  ;
  IF NOT SIG (ei.shelf.failed) THEN
    ;
    CALL log ("Shelf" + $ENCODE (.shelf.no) +" successfully unlocked")
    ;
    ;IF kroset THEN
    ;  SIGNAL k.shelf.pick
    ;END
    ;
    SPEED 200 MM/S
    ACCURACY 0.1
    LMOVE .end
    BREAK
    ;IF kroset THEN
    ;  SIGNAL -k.shelf.pick
    ;END
    ;
    SPEED 50 MM/S
    ACCURACY 0.1
    LMOVE .end + TRANS (-50, 0, 0)
    BREAK
    ;
    SIGNAL -eo.shelf.unlock[.shelf.no]
    SIGNAL -eo.shelf.opened[.shelf.no]
    ;
    ACCURACY 0.1
    LMOVE .end + TRANS (-50, 0, -150)
    ;
    LMOVE #shelf.safe
    BREAK
    ;
    RETURN
  ELSE
    CALL log ("Failed to unlock shelf" + $ENCODE (.shelf.no) +". Task will be stopped")
    SIGNAL s.shelf.failed
    SIGNAL -eo.shelf.unlock[.shelf.no]
    ACCURACY 0.1
    LMOVE .start + TRANS (-50, 0, 0)
    ACCURACY 0.1
    LMOVE .start + TRANS (-50, 0, -50)
    LMOVE .start + TRANS (150, 0, -50)
    JMOVE #shelf.safe
    $safe.flag = "shelf.safe"
    BREAK
    HOME
    HOME2
  END
  ;
.END
.PROGRAM shelf.open (.shelf.no)
  ;
  CALL log ("Open shelf" + $ENCODE (.shelf.no))
  ;
  SPEED 60 ALWAYS
  ACCURACY 5 ALWAYS
  CALL set.tool (3)
  ; gripper.no, time, reverse
  CALL grip.close (1, 0, FALSE)
  CALL grip.close (2, 0, FALSE)
  ;
  POINT .start = shelf.open[.shelf.no, 1]
  POINT .end = shelf.open[.shelf.no, 2]
  ;
  JMOVE #shelf.safe
  BREAK
  $safe.flag = "shelf.safe"
  ;
  LMOVE .start + TRANS (-50, 0, -150)
  LMOVE .start + TRANS (-50, 0, 0)
  $safe.flag = "shelf.work"
  SPEED 50 MM/S
  ACCURACY 0.1
  LMOVE .start
  BREAK
  ; Unlock shelf
  CALL log ("Request unlock shelf" + $ENCODE (.shelf.no))
  SIGNAL eo.shelf.unlock[.shelf.no]
  WAIT BITS (ei.shelf.state[1], 5) <> 0
  ;
  IF NOT SIG (ei.shelf.failed) THEN
    ;
    CALL log ("Shelf" + $ENCODE (.shelf.no) +" successfully unlocked")
    ;
    ;IF kroset THEN
    ;  SIGNAL k.shelf.pick
    ;END
    ;
    SPEED 200 MM/S
    ACCURACY 0.1
    LMOVE .end
    BREAK
    ;IF kroset THEN
    ;  SIGNAL -k.shelf.pick
    ;END
    ;
    SPEED 50 MM/S
    ACCURACY 0.1
    LMOVE .end + TRANS (-50, 0, 0)
    BREAK
    ;
    SIGNAL -eo.shelf.unlock[.shelf.no]
    SIGNAL eo.shelf.opened[.shelf.no]
    ;
    ACCURACY 0.1
    LMOVE .end + TRANS (-50, 0, -50)
    LMOVE .end + TRANS (150, 0, -50)
    ;
    LMOVE #shelf.safe
    $safe.flag = "shelf.safe"
    BREAK
    ;
    RETURN
  ELSE
    CALL log ("Failed to unlock shelf" + $ENCODE (.shelf.no) +". Task will be stopped")
    SIGNAL s.shelf.failed
    SIGNAL -eo.shelf.unlock[.shelf.no]
    ACCURACY 0.1
    LMOVE .start + TRANS (-50, 0, 0)
    ACCURACY 0.1
    LMOVE .start + TRANS (-50, 0, -150)
    JMOVE #shelf.safe
    $safe.flag = "shelf.safe"
    BREAK
    HOME
    HOME2
  END
.END
.PROGRAM shelf.teach ()
    ;
  ; Preaprations for teaching
  TOOL tool.pin
  SIGNAL grip.close[1], -grip.open[1]
  SIGNAL grip.close[2], -grip.open[2]
  JMOVE #shelf.safe
  ;
  ; Teach points for open shelf
  LMOVE shelf.open[hmi.shelf.no, 1] + TRANS (-50, 0, -150)
  LMOVE shelf.open[hmi.shelf.no, 1] + TRANS (-50, 0, 0)
  BREAK
  ; Teach this point for shelf open
  LMOVE shelf.open[hmi.shelf.no, 1] ; **== TEACH POINT ==**
  ; Teach this point for shelf close
  LMOVE shelf.close[hmi.shelf.no, 2] ; **== TEACH POINT ==**
  ;
  LMOVE shelf.open[hmi.shelf.no, 1] + TRANS (-50, 0, 0)
  LMOVE shelf.open[hmi.shelf.no, 1] + TRANS (-50, 0, -150)
  JMOVE #shelf.safe
  ;
  ; Teach points for close shelf
  LMOVE shelf.close[hmi.shelf.no, 1] + TRANS (150, 0, -150)
  LMOVE shelf.close[hmi.shelf.no, 1] + TRANS (-50, 0, -150)
  LMOVE shelf.close[hmi.shelf.no, 1] + TRANS (-50, 0, 0)
  BREAK
  ; Teach this point for shelf close
  LMOVE shelf.close[hmi.shelf.no, 1] ; **== TEACH POINT ==**
  ; Teach this point for shelf open
  LMOVE shelf.open[hmi.shelf.no, 2] ; **== TEACH POINT ==**
  ;
  LMOVE shelf.close[hmi.shelf.no, 1] + TRANS (-50, 0, 0)
  LMOVE shelf.close[hmi.shelf.no, 1] + TRANS (-50, 0, -150)
  LMOVE shelf.close[hmi.shelf.no, 1] + TRANS (150, 0, -150)
  JMOVE #shelf.safe
  ;
.END
.PROGRAM shelf.test ()
  ;
  CALL shelf.open (hmi.shelf.no)
  CALL shelf.close (hmi.shelf.no)
  ;
.END
.PROGRAM Comment___ () ; Comments for IDE. Do not use.
	; @@@ PROJECT @@@
	; @@@ PROJECTNAME @@@
	; LoadWizardPro_
	; @@@ HISTORY @@@
	; @@@ INSPECTION @@@
	; hmi.shelf.no
	; @@@ CONNECTION @@@
	; KROSET R01
	; 127.0.0.1
	; 9105
	; @@@ PROGRAM @@@
	;   Group:Grippers:1
	;     1:grip.open:F
	;       .grip.no 
	;       .time 
	;       .reverse 
	;     1:grip.close:F
	;       .grip.no 
	;       .time 
	;       .reverse 
	;   Group:Plates:2
	;     2:plate.teach:F
	;       .tool.no 
	;       .po 
	;       .px 
	;       .py 
	;       .f 
	;     2:plate.test:F
	;       .tool.no 
	;   Group:Shelves:3
	;     3:shelf.teach:F
	;     3:shelf.test:F
	;     3:shelf.open:F
	;       .shelf.no 
	;     3:shelf.close:F
	;       .shelf.no 
	;   Group:Auxilary:4
	;     4:set.tool:F
	;       .tool.no 
	;     4:a.home:F
	;   Group:Log:5
	;     5:log:F
	;       .$msg 
	;       .i 
	;       .$tmp 
	;     5:log.init:F
	;       .$tmp1 
	;       .$tmp2 
	;       .i 
	;     5:log.pc1:F
	;       .$msg 
	;       .i 
	;       .$tmp 
	;     5:log.clear:F
	;       .i 
	;   0:a.main:F
	;     .$pg.string 
	;   0:pg0:F
	;   Group:Background:6
	;     6:check.speed.pc:B
	;       .speed 
	;     6:check.teach.pc:B
	;     6:check.limits.pc:B
	;   Group:Initialization:7
	;     7:set.switches.pc:B
	;     7:disp.info.pc:B
	;       .$robot.name 
	;       .robot.sn 
	;       .cont.sn 
	;       .$robot.str 
	;       .$cont.str 
	;     7:set.vars.pc:B
	;     7:initialize.pc:B
	;     7:set.io.pc:B
	;   0:autostart.pc:B
	; @@@ TRANS @@@
	; shelf.close[] Shelf close points
	; shelf.open[] Shelf open points
	; @@@ JOINTS @@@
	; #wp.safe[] Safe point for workpiece pick/put
	; #plate.pt.o[] Plate teach points O
	; #plate.pt.x[] Plate teach points X
	; #plate.pt.y[] Plate teach points Y
	; #shelf.safe Safe point above shelves
	; @@@ REALS @@@
	; log.max.count Max log entry count
	; current.tool Current tool number
	; state Robot state
	; hmi.shelf.no Shelf number set on HMI
	; hmi.wp.id Workpiece id on HMI
	; @@@ STRINGS @@@
	; $safe.flag Flag for safety home
	; @@@ INTEGER @@@
	; @@@ SIGNALS @@@
	; ei.robot.speed[] Robot speed from PLC
	; o.debug Robot is in DEBUG mode
	; s.hmi.tool[] Selected tool on HMI
	; s.cnc.chuck[] Selected CNC chuck on HMI
	; s.pr.tch.plate Prime teach plate
	; s.pr.tst.plate Prime test plate
	; s.pr.tch.shelf Prime teach shelf
	; s.pr.tst.shelf Prime test shelf
	; grip.open[] Open gripper
	; grip.close[] Close gripper
	; air.blow.on Air blow on
	; air.blow.off Air blow off
	; grip.opened[] Gripper opened
	; grip.sensor[] Gripper force applied
	; ei.check.grip[] Check gripper sensor
	; eo.process.err Error in process
	; eo.grip.error[] Gripper open error
	; eo.shelf.unlock[] Request shelf unlock
	; ei.shelf.state[] Shelf state is unlocked
	; ei.shelf.failed Fail to unlock shelf
	; eo.shelf.opened[] Shelf is opened
	; s.shelf.failed Internal signal for shelf fail error
	; d.wp.l[] Workpiece lengths
	; d.plt.oy[] Distance between zero point and closest cell by Y
	; d.plt.ox[] Distance between zero point and closest cell by X
	; d.plt.even.dy[] Extrs shift on even rows
	; d.plt.dy[] Distance between cells
	; d.plt.dx[] Distance between rows
	; d.plt.cell.even[] Number of cells in even row on plate
	; d.plt.cell.odd[] Number of cells in odd row on plate
	; d.plt.rows[] Number of rows on plate
	; d.grip.jaw.full[] Gripper jaws full length
	; d.grip.jaw.body[] Gripper jaws body length
	; d.cnc.jaws.full[] CNC jaws full length
	; d.cnc.jaws.body[] CNC jaws body length
	; @@@ TOOLS @@@
	; tool.calib[] 
	; tool.gripper[] 
	; tool.pin 
	; @@@ BASE @@@
	; @@@ FRAME @@@
	; shelf.frame[] 
	; @@@ BOOL @@@
	; @@@ DEFAULTS @@@
	; BASE: NULL
	; TOOL: NULL
	; @@@ WCD @@@
	; SIGNAME: sig1 sig2 sig3 sig4
	; SIGDIM: % % % %
.END
.TRANS
tool.calib[1] -202.800003 0.000000 104.000000 180.000000 90.000008 -180.000000
tool.calib[2] 202.800003 0.000000 104.000000 0.000000 90.000008 -180.000000
tool.gripper[1] -103.300003 0.000000 104.000000 180.000000 90.000008 -180.000000
tool.gripper[2] 103.300003 0.000000 104.000000 0.000000 90.000008 -180.000000
tool.pin 0.000000 112.000000 104.000000 90.000008 90.000008 0.000000
shelf.frame[1,1] 436.998779 637.500610 13.652647 -60.142857 179.999435 -150.142899
shelf.frame[1,2] 436.998840 637.500610 13.652647 -60.142841 179.999435 -150.142899
shelf.close[1,1] 751.995422 712.505249 -3.845108 -89.999977 90.000114 179.999832
shelf.close[1,2] 752.002197 92.508301 -3.839294 -89.999763 90.000267 -179.999237
shelf.open[1,1] 752.002197 92.508301 -3.839294 -89.999763 90.000267 -179.999237
shelf.open[1,2] 751.995422 712.505249 -3.845108 -89.999977 90.000114 179.999832
shelf.close[2,1] 751.995422 712.505249 -183.845108 -89.999977 90.000114 179.999832
shelf.close[2,2] 752.002197 92.508301 -183.839294 -89.999763 90.000267 -179.999237
shelf.open[2,1] 752.002197 92.508301 -183.839294 -89.999763 90.000267 -179.999237
shelf.open[2,2] 751.995422 712.505249 -183.845108 -89.999977 90.000114 179.999832
shelf.close[3,1] 751.995422 712.505249 -363.845108 -89.999977 90.000114 179.999832
shelf.close[3,2] 752.002197 92.508301 -363.839294 -89.999763 90.000267 -179.999237
shelf.open[3,1] 752.002197 92.508301 -363.839294 -89.999763 90.000267 -179.999237
shelf.open[3,2] 751.995422 712.505249 -363.845108 -89.999977 90.000114 179.999832
shelf.close[4,1] 751.995422 712.505249 -543.845108 -89.999977 90.000114 179.999832
shelf.close[4,2] 752.002197 92.508301 -543.839294 -89.999763 90.000267 -179.999237
shelf.open[4,1] 752.002197 92.508301 -543.839294 -89.999763 90.000267 -179.999237
shelf.open[4,2] 751.995422 712.505249 -543.845108 -89.999977 90.000114 179.999832
.END
.JOINTS
#wp.safe[1] 69.568573 8.663290 97.904053 114.625549 -68.547127 10.075710
#wp.safe[2] 69.568573 8.663650 97.904053 114.625549 -68.547127 190.077927
#plate.pt.o[1,1] 63.490707 -38.148388 51.345020 122.649437 -113.012924 13.540280
#plate.pt.o[1,2] 63.490707 -38.148388 51.345020 122.649437 -113.012924 193.540283
#plate.pt.o[2,1] 63.490707 -38.148388 51.345020 122.649437 -113.012924 14.458617
#plate.pt.o[2,2] 63.490707 -38.148388 51.345020 122.649437 -113.012924 193.540283
#plate.pt.o[3,1] 63.490707 -38.148388 51.345020 122.649437 -113.012924 14.458617
#plate.pt.o[3,2] 63.490707 -38.148388 51.345020 122.649437 -113.012924 193.540283
#plate.pt.o[4,1] 63.490707 -38.148388 51.345020 122.649437 -113.012924 14.458617
#plate.pt.o[4,2] 63.490707 -38.148388 51.345020 122.649437 -113.012924 193.540283
#plate.pt.x[1,1] 79.671600 -31.176153 76.969795 121.567230 -89.531021 20.787384
#plate.pt.x[1,2] 79.671600 -31.176153 76.969795 121.567230 -89.531021 200.787399
#plate.pt.x[2,1] 79.671600 -31.176153 76.969795 121.567230 -89.531021 21.211517
#plate.pt.x[2,2] 79.671600 -31.176153 76.969795 121.567230 -89.531021 200.787399
#plate.pt.x[3,1] 79.671600 -31.176153 76.969795 121.567230 -89.531021 21.211517
#plate.pt.x[3,2] 79.671600 -31.176153 76.969795 121.567230 -89.531021 200.787399
#plate.pt.x[4,1] 79.671600 -31.176153 76.969795 121.567230 -89.531021 21.211517
#plate.pt.x[4,2] 79.671600 -31.176153 76.969795 121.567230 -89.531021 200.787399
#plate.pt.y[1,1] 39.390152 4.777491 105.420471 138.215485 -124.432526 40.871090
#plate.pt.y[1,2] 39.390152 4.777491 105.420471 138.215485 -124.432526 220.871109
#plate.pt.y[2,1] 39.390152 4.777491 105.420471 138.215485 -124.432526 41.127808
#plate.pt.y[2,2] 39.390152 4.777491 105.420471 138.215485 -124.432526 220.871109
#plate.pt.y[3,1] 39.390152 4.777491 105.420471 138.215485 -124.432526 41.127808
#plate.pt.y[3,2] 39.390152 4.777491 105.420471 138.215485 -124.432526 220.871109
#plate.pt.y[4,1] 39.390152 4.777491 105.420471 138.215485 -124.432526 41.127808
#plate.pt.y[4,2] 39.390152 4.777491 105.420471 138.215485 -124.432526 220.871109
#shelf.safe 77.267387 4.813991 100.780472 34.271633 76.990814 99.275299
.END
.REALS
log.max.count = 512
current.tool = 0
ei.robot.speed[0] = 1145
o.debug = 78
state = 0
s.hmi.tool[1] = 2102
s.hmi.tool[2] = 2103
s.cnc.chuck[1] = 2104
s.cnc.chuck[2] = 2105
s.pr.tch.plate = 2106
s.pr.tst.plate = 2107
s.pr.tch.shelf = 2108
s.pr.tst.shelf = 2109
hmi.shelf.no = 1
grip.open[1] = 33
grip.open[2] = 35
grip.close[1] = 34
grip.close[2] = 36
air.blow.on = 37
air.blow.off = 38
grip.opened[1] = 1033
grip.opened[2] = 1034
grip.sensor[1] = 1035
grip.sensor[2] = 1036
ei.check.grip[1] = 1084
ei.check.grip[2] = 1092
eo.process.err = 115
eo.grip.error[1] = 84
eo.grip.error[2] = 92
eo.shelf.unlock[1] = 133
eo.shelf.unlock[2] = 134
eo.shelf.unlock[3] = 135
eo.shelf.unlock[4] = 136
ei.shelf.state[1] = 1133
ei.shelf.state[2] = 1134
ei.shelf.state[3] = 1135
ei.shelf.state[4] = 1136
ei.shelf.failed = 1137
eo.shelf.opened[1] = 129
eo.shelf.opened[2] = 130
eo.shelf.opened[3] = 131
eo.shelf.opened[4] = 132
s.shelf.failed = 2010
hmi.wp.id = 0
d.wp.l[0,0] = 1321
d.wp.l[1,0] = 1329
d.plt.oy[0] = 1497
d.wp.l[2,0] = 1337
d.plt.ox[0] = 1481
d.plt.even.dy[0] = 1465
d.plt.dy[0] = 1449
d.plt.dx[0] = 1433
d.plt.cell.even[0] = 1417
d.plt.cell.odd[0] = 1413
d.plt.rows[0] = 1409
d.grip.jaw.full[1,0] = 1345
d.grip.jaw.full[2,0] = 1361
d.grip.jaw.body[1,0] = 1353
d.grip.jaw.body[2,0] = 1369
d.cnc.jaws.full[1,0] = 1377
d.cnc.jaws.full[2,0] = 1393
d.cnc.jaws.body[1,0] = 1385
d.cnc.jaws.body[2,0] = 1401
.END
.STRINGS
$safe.flag = ""
.END
