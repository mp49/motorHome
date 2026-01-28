# motorHome

EPICS database template files to provide a set of records to aid in homing
any motor record axis. Several homing methods (H_TYPE) are supported:

1) Custom home routine
2) Home by using the motor record HOMF/HOMR
3) Home to a low or high limit switch (direction is defined by H_LIM_DIR)
4) Home by just setting the home position at the current position
5) Home by just setting the motor record offset OFF

Method 2) above covers the situations where the controller or driver 
takes care of the homing, for example using a home switch or encoder 
reference mark.
 
Method 5) should be used when we are using absolute encoders. In case
the controller or driver doesn't support offseting the position, the only
way to change the position is to use the OFF field.
 
The template file `motorHome.template` instantiates all the common records, and all the 
pre-defined sequences. It also defines a top level 'Home' record
which then links to the choosen sequence, which provides a common
interface for the different sequences. 

`motorHome.template` is used like following example substitutions file entry:
```
file motorHome.template
{
pattern {M,                        H,  H_TYPE, H_POS,   H_BACKOFF, H_SPEED, H_EGU, H_LIM_DIR}
        {BL99:Mot:Motor1,        :H:, 3,      0.0,     1.0,       0.1,     mm,    0}
        {BL99:Mot:Motor2,        :H:, 3,      0.0,     1.0,       0.1,     mm,    0}
        {BL99:Mot:Motor3,        :H:, 3,      0.0,     1.0,       0.1,     mm,    0}
}
```
The above example defines a home routine for 3 motors, each of them will home to a limit
switch (`H_TYPE=3`) and define 0.0 (`H_POS`). The routine will first find the limit switch, by widening 
the software limits and moving to a limit. Then backoff a distance H_BACKOFF, then move back on the 
switch at speed `H_SPEED`, then set `H_POS`. Other records can be used at runtime to defined pre and post
home behavior (see the OPI screens).

The macros used by `motorHome.template` are:
```
 Macros:
 M -          Base PV name (should match the motor record)
 H -          Second part of the PV names (eg. :Home: or simply :H:)
 H_TYPE -     Home type (see $(M)$(H)Type record in motorRecordCommon.template). Options are:
                0 - None (no home routine)
                1 - Custom home routine
                2 - Motor record (HOMF/HOMR) based home routine
                3 - Limit switch routine (pos or neg, defined by H_LIM_DIR below)
                4 - Change the position to the home position (H_POS) by setting the controller position
                5 - Change the position to the home position (H_POS) by using the offset field 
                    (use this for absolute encoders)
 H_POS -      Home position to set at end of home sequence. 
 H_SPEED -    Home speed. This is used when approching a limit switch (after we initially found 
              it and have already backed off the switch)
 H_BACKOFF -  Home backoff. The distance to move off a limit switch. The sign is important.
 H_EGU -      Units string (default=mm)
 H_PREC -     Display precision (default=4)
 H_LIM_DIR -  Define which limit switch to move to for both the limit switch homes and the motor record homes.
              Default = 0. 0=Neg Limit, 1=Pos Limit
 H_ASG -      The ASG level to use for the records that configure the home routines (default=EXPERT)
 H_ASG_MOVE - The ASG level to use for the move records (the main Home record included) (default=)
```

It would also be possible to just define the sequence that you want 
to use by using the motorRecordCommon.template and one of the pre-defined 
sequences. In this case you would have to use the PV name specific for
the pre-defined sequence that you choose, or define your own top level 'Home' record.




