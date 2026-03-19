# RAPID-tools
list of several ABB RAPID functions

------

ModuleCircle :

Provide function to interpolate a target on circle arc.
Takes as input start target, mid target, end target, and fraction.
It will return a new target at the fraction of the arc.
This is usefull e.g. to "split" a circle arc in two circle arc

Example usage : 

    CONST robtarget Target_start:=[[205.062875892,284.174650089,218.889882592],[0.071748026,-0.453950528,0.887374762,0.036703832],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_cir:=[[340.658020169,82.207834888,218.889882592],[0.080027061,-0.117736059,0.989769301,0.009519461],[0,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_end:=[[345.355023779,-59.463622266,218.889882592],[0.098764324,-0.166265834,0.973106821,0.125157481],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    PERS robtarget Target_25:=[[275.406,216.697,218.89],[0.0792841,-0.385175,0.917504,0.0595107],[0,-1,0,0],[0,0,0,0,0,0]];
    PERS robtarget Target_50:=[[324.441,132.455,218.89],[0.0863275,-0.314005,0.94193,0.0819477],[0,-1,0,0],[0,0,0,0,0,0]];
    PERS robtarget Target_75:=[[348.374,37.9641,218.89],[0.0928344,-0.240884,0.960503,0.103875],[0,-1,0,0],[0,0,0,0,0,0]];
    
    PROC Main()
        CalculateTargets;
        MoveL Target_start,v100,fine,tool0\WObj:=wobj0;
        MoveC Target_25,Target_50,v100,fine,tool0\WObj:=wobj0;
        WaitTime 1;
        MoveC Target_75,Target_end,v100,fine,tool0\WObj:=wobj0;
    ENDPROC
    
    PROC CalculateTargets()
        Target_25:=InterpolateArc(Target_start,Target_cir,Target_end,0.25);
        Target_50:=InterpolateArc(Target_start,Target_cir,Target_end,0.5);
        Target_75:=InterpolateArc(Target_start,Target_cir,Target_end,0.75);
        !
    ENDPROC

------

ModuleValidMoveL :

Provide function to verify if MoveL motion will result in joint limits or out of reach.
Takes as input "from" position, "to" position, "tooldata" and "workobject" coordinate systems.
It will compute line interpolation and verif at each step if there is a joint limit, out of reach or singularity position.
Return TRUE is the MoveL is valid, FALSE otherwize.
This is usefull to check in advance if e.g a calculated position is reachable in a linear motion.
This function might take time for "long" distances, in that case it is recommended to increase interpolation step (0.1 mm / 0.1 deg default)

Example usage : 
    
    PERS robtarget p10:=[[0.281947,1.52626,12.7066],[0.861263,-0.0524068,-0.497248,-0.0906863],[-1,0,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    PERS robtarget p20:=[[0.27012,1.52975,12.6802],[0.866025,-0.000000244,-0.5,0.000000264],[-1,0,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    PERS robtarget p30:=[[0.270078861,1.529731591,12.680164984],[0.693371277,0.512056592,-0.411477477,-0.296176646],[-1,1,-2,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    PERS robtarget p40:=[[10.889682783,651.437825272,15.266341984],[0.693371301,0.512056593,-0.411477427,-0.296176656],[0,1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    
    VAR bool bMoveLOk;
    
    PROC Main()
        MoveJ p10,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        bMoveLOk:=ValidMoveL(p10,p20,Tooldata_2_2,Workobject_2);
        IF(bMoveLOk=TRUE) THEN
            MoveL p20,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        ELSE
            MoveJ p20,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        ENDIF
        bMoveLOk:=ValidMoveL(p20,p30,Tooldata_2_2,Workobject_2);
        IF(bMoveLOk=TRUE) THEN
            MoveL p30,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        ELSE
            MoveJ p30,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        ENDIF
        bMoveLOk:=ValidMoveL(p30,p40,Tooldata_2_2,Workobject_2);
        IF(bMoveLOk=TRUE) THEN
            MoveL p40,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        ELSE
            MoveJ p40,v100,fine,Tooldata_2_2\WObj:=Workobject_2;
        ENDIF
        !
    ENDPROC
    
