# RAPID-tools
list of several ABB RAPID functions

ModuleCircle :
Provide function to interpolate a target on circle arc.
Takes as input start target, mid target, end target, and fraction.
It will return a new target at the fraction of the arc.

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
