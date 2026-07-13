SetFactory("OpenCASCADE");

// 1. Rectangles (Distinct entities)
Rectangle(1) = {0, 0, 0, 1.5, 0.75};
Rectangle(2) = {0, 0.75, 0, 1.5, 0.75};
Rectangle(3) = {3, 0.75, 0, 1.5, 0.75};
Rectangle(4) = {3, 0, 0, 1.5, 0.75};

// 2. The U-Bend (This results in exactly ONE surface entity)
Disk(5) = {2.25, 1.5, 0, 2.25, 2.25};
Disk(6) = {2.25, 1.5, 0, 0.75, 0.75};
BooleanDifference(9) = { Surface{5}; Delete; }{ Surface{6}; Delete; };
Rectangle(8) = {-1, -2, 0, 7, 3.5}; 
BooleanDifference(10) = { Surface{9}; Delete; }{ Surface{8}; Delete; };

// 3. The Stitching
// This fixes the mesh "bleeding" by sharing nodes, 
// but it PRESERVES your lines and separate surfaces.
BooleanFragments{ Surface{1, 2, 3, 4, 10}; Delete; }{ }

Physical Curve("stefan", 17) = {3, 8};
//+
Physical Curve("control", 18) = {1, 12};
//+
Physical Surface("solid", 19) = {1, 4};
//+
Physical Surface("gas", 20) = {2, 10, 3};

Transfinite Curve {16, 15} = 50;
Transfinite Curve {10, 6} = 50 Using Bump 0.1;
Transfinite Surface {10};
Recombine Surface {10};

Transfinite Curve {3} = 50;
Transfinite Curve {5} = 50 Using Progression 1.1;
Transfinite Curve {-7} = 50 Using Progression 1.1;
Transfinite Surface {2};
Recombine Surface {2};

Transfinite Curve {3, 1} = 50 Using Bump 0.1;
Transfinite Curve {4} = 50 Using Progression 1.1;
Transfinite Curve {-2} = 50 Using Progression 1.1;
Transfinite Surface {1};
Recombine Surface {1};

Transfinite Curve {8} = 50;
Transfinite Curve {9} = 50 Using Progression 1.1;
Transfinite Curve {-11} = 50 Using Progression 1.1;
Transfinite Surface {3};
Recombine Surface {3};

Transfinite Curve {8, 12} = 50 Using Bump 0.1;
Transfinite Curve {-13} = 50 Using Progression 1.1;
Transfinite Curve {14} = 50 Using Progression 1.1;
Transfinite Surface {4};
Recombine Surface {4};
