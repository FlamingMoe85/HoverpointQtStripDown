The code originates from the Qt example project "Gradients", where everything else other than required for handling hoverdots on a Shadewidget area was stripped. 
So all mechanims active here, are originating from that project. 

A minimal project, which lets the user drag, add and delete dots on an area. 

The class doing that is "Shadewidgets" which uses "Hoverpoints". 
ShadeWidget is the widget to be painted on and holds a reference to an instace of "Hoverpoints". 
Hoverpoints hold a member of type QPolygonF named m_points which is a vector of QPointF. This vector represent the numerical values of the dots to be displayed. 
The rest of class Hoverpoints is logic to work on this vector - especially handling several events from mouse in method "eventFilter", to drag, add and delete dots. 
The logic also takes care to keep the dots in "m_points" in order, by triggering signal firePointChange(), which end ins slot pointsUpdated() in gradients.cpp, 
where a sort function opperates on the points.  


One change is introduced though. In the original code the dots are representing the absolute pixel positions. That means, when the window is resized, the dots change 
thier value. 
This is changed to relative positions ranging between
0.0 and 1.0
That means, when resizing the window, the values of the dots remain the same. Although on first resize currently an adjustment happens. Needs to be figured out. 
-
In order to make use of the original logic, methods for translating between relativ and absolute positions are existing, which use the boundingRect() method to get a 
rectangle representing the graphical area to be drawn on. 
The translation methods are
    qreal TranslateRelToAbsX(qreal x);
    qreal TranslateRelToAbsY(qreal y);
    QPointF TranslateAbsToRel(qreal x, qreal y);
    QPointF TranslateRelToAbs(qreal x, qreal y);

In principle it would also be possible to leave everything as it was and just calculate a second QPolygonF with relative numbers, but that would also mean, there would systematicaly
be changes in that second Polygon due to roundings between int and float, because in that concept m_points are rather a representation of what is displayed and with the introduced 
approach the grapic is a representation of the actual data in m_points. 
