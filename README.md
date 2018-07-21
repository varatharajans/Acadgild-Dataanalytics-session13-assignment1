# Acadgild-Dataanalytics-session13-assignment1
DATA ANALYTICS WITH R, EXCEL AND TABLEAU SESSION 13 ASSIGNMENT 1
session13 assignment1
varatharajan
July 18, 2018
#a. Find out top 5 attributes having highest correlation (select only Numeric features).
#b. Find out top 3 reasons for having more crime in a city.
#c. Which all attributes have correlation with crime rate?

COBRA_YTD2017<-read.csv('C:/Users/seshan/Desktop/COBRA-YTD2017.csv')
require(Amelia)
## Loading required package: Amelia
## Loading required package: Rcpp
## ## 
## ## Amelia II: Multiple Imputation
## ## (Version 1.7.5, built: 2018-05-07)
## ## Copyright (C) 2005-2018 James Honaker, Gary King and Matthew Blackwell
## ## Refer to http://gking.harvard.edu/amelia/ for more information
## ##
library(Rcpp)
data<-COBRA_YTD2017
data[4:10,3] <- rep(NA,7)
data[1:5,4] <- NA
data <- data[-c(5,6)]
summary(data)
##     MI_PRINX         offense_id              rpt_date    
##  Min.   :8838438   Min.   :1.608e+08   7/26/2017 :  106  
##  1st Qu.:8904204   1st Qu.:1.711e+08   10/16/2017:  103  
##  Median :8910894   Median :1.720e+08   11/1/2017 :  103  
##  Mean   :8910851   Mean   :6.523e+08   9/21/2017 :  101  
##  3rd Qu.:8917584   3rd Qu.:1.728e+08   11/28/2017:  100  
##  Max.   :8924410   Max.   :1.730e+11   (Other)   :26239  
##                                        NA's      :    7  
##       occur_date       poss_time          beat       apt_office_prefix
##  11/17/2017:  110   8:00:00 :  526   Min.   :101.0          :26213    
##  10/7/2017 :  106   7:00:00 :  430   1st Qu.:208.0   APT    :  314    
##  8/19/2017 :  105   12:00:00:  426   Median :312.0   STE    :   25    
##  10/28/2017:  102   10:00:00:  376   Mean   :355.6   ROOM   :   21    
##  10/31/2017:   99   9:00:00 :  376   3rd Qu.:505.0   BLDG   :   12    
##  (Other)   :26232   16:00:00:  375   Max.   :710.0   UNIT   :   12    
##  NA's      :    5   (Other) :24250                   (Other):  162    
##  apt_office_num                                      location    
##         :22133   1801 HOWELL MILL RD NW                  :  142  
##  A      :  120   3393 PEACHTREE RD NE @LENOX MALL        :  140  
##  B      :  108   1275 CAROLINE ST NE @TARGET - CAROLINE  :  136  
##  1      :   61   3393 PEACHTREE RD NE                    :  129  
##  2      :   48   835 MARTIN L KING JR DR NW              :  108  
##  5      :   46   2841 GREENBRIAR PKWY SW @GREENBRIAR MALL:   95  
##  (Other): 4243   (Other)                                 :26009  
##     MinOfucr     MinOfibr_code    dispo_code    MaxOfnum_victims
##  Min.   :110.0   2305   :9024          :22959   Min.   : 0.00   
##  1st Qu.:521.0   2404   :2774   10     : 2893   1st Qu.: 1.00   
##  Median :640.0   2303   :2486   20     :  632   Median : 1.00   
##  Mean   :598.8   2399   :1946   30     :  210   Mean   : 1.16   
##  3rd Qu.:660.0   2202   :1802   40     :   36   3rd Qu.: 1.00   
##  Max.   :730.0   2308   :1381   60     :   20   Max.   :27.00   
##                  (Other):7346   (Other):    9   NA's   :75      
##   Shift         Avg.Day        loc_type                   UC2.Literal  
##  Day :6882   Sat    :3713   Min.   : 1.00   LARCENY-FROM VEHICLE:9840  
##  Eve :9151   Sun    :3569   1st Qu.:13.00   LARCENY-NON VEHICLE :6589  
##  Morn:7014   Tue    :3542   Median :18.00   AUTO THEFT          :3197  
##  Unk :3712   Wed    :3539   Mean   :20.76   BURGLARY-RESIDENCE  :2635  
##              Mon    :3492   3rd Qu.:20.00   AGG ASSAULT         :2024  
##              Thu    :3455   Max.   :99.00   ROBBERY-PEDESTRIAN  :1126  
##              (Other):5449   NA's   :3344    (Other)             :1348  
##             neighborhood        npu              x         
##  Downtown         : 1828   M      : 3077   Min.   :-84.55  
##  Midtown          : 1410   E      : 2742   1st Qu.:-84.43  
##                   : 1185   B      : 2716   Median :-84.40  
##  Old Fourth Ward  :  697   D      : 1281   Mean   :-83.69  
##  Lindbergh/Morosgo:  595   V      : 1281   3rd Qu.:-84.37  
##  West End         :  571   T      : 1140   Max.   :  0.00  
##  (Other)          :20473   (Other):14522                   
##        y        
##  Min.   : 0.00  
##  1st Qu.:33.73  
##  Median :33.76  
##  Mean   :33.47  
##  3rd Qu.:33.79  
##  Max.   :33.88  
## 
str(COBRA_YTD2017)
## 'data.frame':    26759 obs. of  23 variables:
##  $ MI_PRINX         : int  8924155 8924156 8924157 8924158 8924159 8924160 8924161 8924162 8924163 8924164 ...
##  $ offense_id       : num  1.74e+08 1.74e+08 1.74e+08 1.74e+08 1.74e+08 ...
##  $ rpt_date         : Factor w/ 365 levels "1/1/2017","1/10/2017",..: 117 117 117 117 117 117 117 117 117 117 ...
##  $ occur_date       : Factor w/ 471 levels "1/1/2008","1/1/2015",..: 174 145 174 174 176 174 176 176 174 176 ...
##  $ occur_time       : Factor w/ 1355 levels "","0:00:00","0:01:00",..: 955 290 883 763 43 940 112 2 2 2 ...
##  $ poss_date        : Factor w/ 412 levels "1/1/2015","1/1/2017",..: 147 145 147 147 147 147 147 147 147 147 ...
##  $ poss_time        : Factor w/ 1434 levels "","0:00:00","0:01:00",..: 32 902 62 68 50 88 121 722 1024 1056 ...
##  $ beat             : int  510 501 303 507 409 612 605 603 605 304 ...
##  $ apt_office_prefix: Factor w/ 88 levels "","#8","1","10",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ apt_office_num   : Factor w/ 2044 levels "","#5","]","`",..: 1 1 1 1 1 1 213 1 1 1372 ...
##  $ location         : Factor w/ 13865 levels ": 565 Main St NE",..: 9394 1133 10955 7860 5557 1525 8250 9706 9456 455 ...
##  $ MinOfucr         : int  640 640 640 640 640 650 311 640 640 531 ...
##  $ MinOfibr_code    : Factor w/ 68 levels "","1101","1101A",..: 51 51 51 51 51 50 30 51 51 42 ...
##  $ dispo_code       : Factor w/ 8 levels "","10","20","30",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ MaxOfnum_victims : int  2 1 1 1 2 1 1 1 1 1 ...
##  $ Shift            : Factor w/ 4 levels "Day","Eve","Morn",..: 3 4 3 2 3 3 3 3 4 3 ...
##  $ Avg.Day          : Factor w/ 8 levels "Fri","Mon","Sat",..: 3 7 3 3 4 4 4 4 3 4 ...
##  $ loc_type         : int  13 13 18 18 18 18 26 18 13 26 ...
##  $ UC2.Literal      : Factor w/ 11 levels "AGG ASSAULT",..: 6 6 6 6 6 6 10 6 6 4 ...
##  $ neighborhood     : Factor w/ 239 levels "","Adair Park",..: 80 117 145 64 3 83 103 164 103 175 ...
##  $ npu              : Factor w/ 26 levels "","A","B","C",..: 14 6 22 14 19 23 23 14 23 22 ...
##  $ x                : num  -84.4 -84.4 -84.4 -84.4 -84.5 ...
##  $ y                : num  33.8 33.8 33.7 33.8 33.7 ...
library(car)
## Loading required package: carData
fit<-lm(beat~MinOfucr+  MaxOfnum_victims+loc_type+neighborhood+x+y,data =COBRA_YTD2017)
fit
## 
## Call:
## lm(formula = beat ~ MinOfucr + MaxOfnum_victims + loc_type + 
##     neighborhood + x + y, data = COBRA_YTD2017)
## 
## Coefficients:
##                               (Intercept)  
##                                 3.088e+02  
##                                  MinOfucr  
##                                 2.221e-02  
##                          MaxOfnum_victims  
##                                -2.153e-01  
##                                  loc_type  
##                                -1.020e-01  
##                    neighborhoodAdair Park  
##                                -5.594e+01  
##                    neighborhoodAdams Park  
##                                -1.071e+01  
##                    neighborhoodAdamsville  
##                                -1.072e+02  
##                   neighborhoodAlmond Park  
##                                -1.922e+02  
##                  neighborhoodAmal Heights  
##                                -8.001e+01  
##                   neighborhoodAnsley Park  
##                                 2.682e+02  
##               neighborhoodArden/Habersham  
##                                 3.451e+01  
##                       neighborhoodArdmore  
##                                -1.492e+01  
##                neighborhoodArgonne Forest  
##                                 3.535e+01  
##             neighborhoodArlington Estates  
##                                -9.771e+01  
##                 neighborhoodAshley Courts  
##                                -4.434e+01  
##               neighborhoodAshview Heights  
##                                -2.293e+02  
##                   neighborhoodAtkins Park  
##                                 3.590e+02  
##       neighborhoodAtlanta Industrial Park  
##                                -1.911e+02  
##     neighborhoodAtlanta University Center  
##                                -2.148e+02  
##              neighborhoodAtlantic Station  
##                                 2.518e+02  
##                neighborhoodAudobon Forest  
##                                 4.003e+00  
##           neighborhoodAudobon Forest West  
##                                -1.011e+01  
##                   neighborhoodBaker Hills  
##                                 2.365e+01  
##                  neighborhoodBakers Ferry  
##                                 3.247e+00  
##                      neighborhoodBankhead  
##                                -1.978e+02  
##               neighborhoodBankhead/Bolton  
##                                -2.120e+02  
##                 neighborhoodBeecher Hills  
##                                 1.716e+01  
##                      neighborhoodBen Hill  
##                                -1.239e+02  
##                neighborhoodBen Hill Acres  
##                                -8.011e+01  
##               neighborhoodBen Hill Forest  
##                                -1.018e+02  
##                neighborhoodBen Hill Pines  
##                                -9.275e+01  
##              neighborhoodBen Hill Terrace  
##                                -7.799e+01  
##                  neighborhoodBenteen Park  
##                                 2.536e+02  
##                 neighborhoodBerkeley Park  
##                                -3.422e+01  
##                neighborhoodBetmar LaVilla  
##                                -8.306e+01  
##       neighborhoodBlair Villa/Poole Creek  
##                                -1.533e+02  
##                     neighborhoodBlandtown  
##                                -5.819e+01  
##                        neighborhoodBolton  
##                                -3.747e+01  
##                  neighborhoodBolton Hills  
##                                -1.732e+02  
##                  neighborhoodBoulder Park  
##                                -2.017e+00  
##             neighborhoodBoulevard Heights  
##                                 2.708e+02  
##                       neighborhoodBrandon  
##                                -5.773e+00  
##                     neighborhoodBrentwood  
##                                -1.044e+02  
##                    neighborhoodBriar Glen  
##                                -6.678e+01  
##                    neighborhoodBrookhaven  
##                                 1.103e+02  
##             neighborhoodBrookview Heights  
##                                -2.056e+02  
##                     neighborhoodBrookwood  
##                                -1.984e+01  
##               neighborhoodBrookwood Hills  
##                                -1.158e+01  
##              neighborhoodBrowns Mill Park  
##                                -1.083e+02  
##               neighborhoodBuckhead Forest  
##                                 6.822e+01  
##              neighborhoodBuckhead Heights  
##                                 8.416e+01  
##              neighborhoodBuckhead Village  
##                                 5.630e+01  
##                 neighborhoodBush Mountain  
##                                 2.443e+01  
##                   neighborhoodButner/Tell  
##                                -1.055e+02  
##                   neighborhoodCabbagetown  
##                                 3.089e+02  
##              neighborhoodCampbellton Road  
##                                -2.648e+01  
##                  neighborhoodCandler Park  
##                                 3.534e+02  
##               neighborhoodCapitol Gateway  
##                                 2.862e+02  
##                  neighborhoodCapitol View  
##                                -8.205e+01  
##            neighborhoodCapitol View Manor  
##                                -7.716e+01  
##                    neighborhoodCarey Park  
##                                -1.900e+02  
##               neighborhoodCarroll Heights  
##                                -2.290e+02  
##                  neighborhoodCarver Hills  
##                                -1.664e+02  
##           neighborhoodCascade Avenue/Road  
##                                 1.491e+01  
##                 neighborhoodCascade Green  
##                                -3.778e+01  
##               neighborhoodCascade Heights  
##                                -1.374e+01  
##              neighborhoodCastleberry Hill  
##                                 1.847e+02  
##                    neighborhoodCastlewood  
##                                 2.267e+01  
##                   neighborhoodCenter Hill  
##                                -2.125e+02  
##                  neighborhoodChalet Woods  
##                                 3.127e+01  
##               neighborhoodChanning Valley  
##                                -2.311e+01  
##                 neighborhoodChastain Park  
##                                 9.584e+01  
##                neighborhoodChosewood Park  
##                                -4.798e+01  
##               neighborhoodCollier Heights  
##                                -2.200e+02  
##                 neighborhoodCollier Hills  
##                                -1.576e+01  
##           neighborhoodCollier Hills North  
##                                -7.174e+00  
##                neighborhoodColonial Homes  
##                                -1.751e+00  
##                   neighborhoodCross Creek  
##                                -1.726e+01  
##        neighborhoodCuster/McDonough/Guice  
##                                 2.544e+02  
##                      neighborhoodDeerwood  
##                                -9.579e+01  
##                   neighborhoodDixie Hills  
##                                -2.329e+02  
##                      neighborhoodDowntown  
##                                 2.113e+02  
##                   neighborhoodDruid Hills  
##                                 3.727e+02  
##              neighborhoodEast Ardley Road  
##                                -1.065e+01  
##                  neighborhoodEast Atlanta  
##                                 3.081e+02  
##            neighborhoodEast Chastain Park  
##                                 1.171e+02  
##                     neighborhoodEast Lake  
##                                 3.528e+02  
##                      neighborhoodEdgewood  
##                                 3.384e+02  
##                 neighborhoodElmco Estates  
##                                -9.468e+01  
##                neighborhoodEnglish Avenue  
##                                -1.822e+02  
##                  neighborhoodEnglish Park  
##                                -1.962e+02  
##                      neighborhoodFairburn  
##                                -8.784e+01  
##              neighborhoodFairburn Heights  
##                                -2.392e+02  
##                 neighborhoodFairburn Mays  
##                                -2.608e-01  
##   neighborhoodFairburn Road/Wisteria Lane  
##                                 1.013e+01  
##                 neighborhoodFairburn Tell  
##                                -9.799e+01  
##                 neighborhoodFairway Acres  
##                                -1.051e+02  
##                      neighborhoodFernleaf  
##                                -2.435e+01  
##               neighborhoodFlorida Heights  
##                                -1.341e+02  
##                neighborhoodFort McPherson  
##                                -1.178e+00  
##                   neighborhoodFort Valley  
##                                -1.903e+01  
##                  neighborhoodGarden Hills  
##                                 4.575e+01  
##                  neighborhoodGeorgia Tech  
##                                 2.323e+02  
##              neighborhoodGlenrose Heights  
##                                -1.316e+02  
##                    neighborhoodGrant Park  
##                                 2.886e+02  
##            neighborhoodGreen Acres Valley  
##                                -1.467e+01  
##            neighborhoodGreen Forest Acres  
##                                -8.231e+00  
##                    neighborhoodGreenbriar  
##                                -8.013e+01  
##            neighborhoodGreenbriar Village  
##                                -7.815e+01  
##                    neighborhoodGrove Park  
##                                -2.037e+02  
##                  neighborhoodHammond Park  
##                                -1.334e+02  
##                  neighborhoodHanover West  
##                                -1.605e+01  
##               neighborhoodHarland Terrace  
##                                 3.398e+00  
##                 neighborhoodHarris Chiles  
##                                -2.319e+02  
##        neighborhoodHarvel Homes Community  
##                                -2.362e+02  
##               neighborhoodHeritage Valley  
##                                -5.375e+01  
##                    neighborhoodHigh Point  
##                                -6.895e+01  
##                    neighborhoodHills Park  
##                                -5.258e+01  
##                     neighborhoodHome Park  
##                                 2.364e+02  
##           neighborhoodHorseshoe Community  
##                                -2.681e+01  
##                  neighborhoodHunter Hills  
##                                -2.145e+02  
##                    neighborhoodHuntington  
##                                -1.373e+02  
##                    neighborhoodInman Park  
##                                 3.329e+02  
##                     neighborhoodIvan Hill  
##                                 1.963e+01  
##                       neighborhoodJoyland  
##                                -7.231e+01  
##                       neighborhoodJust Us  
##                                -2.265e+02  
##                  neighborhoodKings Forest  
##                                -7.272e+01  
##                     neighborhoodKingswood  
##                                 4.664e+01  
##                      neighborhoodKirkwood  
##                                 3.499e+02  
##    neighborhoodKnight Park/Howell Station  
##                                -1.739e+02  
##                   neighborhoodLake Claire  
##                                 3.634e+02  
##                  neighborhoodLake Estates  
##                                -1.091e+02  
##                      neighborhoodLakewood  
##                                -8.625e+01  
##              neighborhoodLakewood Heights  
##                                -7.485e+01  
##                neighborhoodLaurens Valley  
##                                -3.553e+01  
##                  neighborhoodLeila Valley  
##                                -8.211e+01  
##                         neighborhoodLenox  
##                                 8.408e+01  
##                 neighborhoodLincoln Homes  
##                                -1.737e+02  
##             neighborhoodLindbergh/Morosgo  
##                                 4.112e+01  
##        neighborhoodLindridge/Martin Manor  
##                                 4.212e+01  
##                neighborhoodLoring Heights  
##                                -3.741e+01  
##                  neighborhoodMagnum Manor  
##                                -1.892e+01  
##             neighborhoodMargaret Mitchell  
##                                -3.024e+00  
##        neighborhoodMarietta Street Artery  
##                                 2.257e+02  
##                          neighborhoodMays  
##                                 8.219e+00  
##            neighborhoodMeadowbrook Forest  
##                                -7.720e+01  
##                neighborhoodMechanicsville  
##                                -3.398e+01  
##                      neighborhoodMellwood  
##                                -2.445e+02  
##                 neighborhoodMemorial Park  
##                                -4.356e+00  
##                       neighborhoodMidtown  
##                                 2.483e+02  
##               neighborhoodMidwest Cascade  
##                                -3.968e+01  
##                neighborhoodMonroe Heights  
##                                -1.826e+02  
##        neighborhoodMorningside/Lenox Park  
##                                 1.177e+01  
##                   neighborhoodMozley Park  
##                                -2.374e+02  
##              neighborhoodMt. Gilead Woods  
##                                -6.112e+01  
##             neighborhoodMt. Paran Parkway  
##                                 8.077e+01  
##           neighborhoodMt. Paran/Northside  
##                                 7.260e+01  
##                   neighborhoodNiskey Cove  
##                                -6.749e+01  
##                   neighborhoodNiskey Lake  
##                                -5.861e+01  
##                neighborhoodNorth Buckhead  
##                                 8.552e+01  
##                 neighborhoodNorwood Manor  
##                                -7.743e+01  
##                      neighborhoodOakcliff  
##                                -2.579e+02  
##                       neighborhoodOakland  
##                                 2.976e+02  
##                  neighborhoodOakland City  
##                                 1.604e+01  
##          neighborhoodOld Fairburn Village  
##                                -4.514e+01  
##               neighborhoodOld Fourth Ward  
##                                 3.293e+02  
##                    neighborhoodOld Gordon  
##                                -2.503e+02  
##                  neighborhoodOrchard Knob  
##                                -1.258e+02  
##                 neighborhoodOrmewood Park  
##                                 2.985e+02  
##                         neighborhoodPaces  
##                                 2.661e+01  
##     neighborhoodPeachtree Battle Alliance  
##                                 6.536e+00  
##        neighborhoodPeachtree Heights East  
##                                 3.028e+01  
##        neighborhoodPeachtree Heights West  
##                                 4.196e+01  
##               neighborhoodPeachtree Hills  
##                                 2.127e+01  
##                neighborhoodPeachtree Park  
##                                 6.859e+01  
##            neighborhoodPenelope Neighbors  
##                                -2.376e+02  
##                   neighborhoodPeoplestown  
##                                -4.417e+01  
##                     neighborhoodPerkerson  
##                                -1.263e+02  
##                 neighborhoodPeyton Forest  
##                                 2.126e+01  
##              neighborhoodPiedmont Heights  
##                                 7.225e+00  
##                    neighborhoodPine Hills  
##                                 6.717e+01  
##                    neighborhoodPittsburgh  
##                                -5.171e+01  
##                 neighborhoodPleasant Hill  
##                                 3.884e+01  
##                    neighborhoodPolar Rock  
##                                -9.784e+01  
##                   neighborhoodPomona Park  
##                                -1.539e+01  
##               neighborhoodPoncey-Highland  
##                                 3.494e+02  
##               neighborhoodPrinceton Lakes  
##                                -1.367e+02  
##                  neighborhoodRandall Mill  
##                                 3.644e+01  
##           neighborhoodRebel Valley Forest  
##                                -8.822e+01  
##                  neighborhoodReynoldstown  
##                                 3.229e+02  
##             neighborhoodRidgecrest Forest  
##                                -3.635e-01  
##                neighborhoodRidgedale Park  
##                                 9.914e+01  
##             neighborhoodRidgewood Heights  
##                                -2.189e+01  
##                     neighborhoodRiverside  
##                                -6.176e+01  
##                      neighborhoodRockdale  
##                                -1.745e+02  
##              neighborhoodRosedale Heights  
##                                -1.086e+02  
##                     neighborhoodRue Royal  
##                                -8.399e+01  
##            neighborhoodSandlewood Estates  
##                                -8.392e+01  
##               neighborhoodScotts Crossing  
##                                -1.555e+02  
##               neighborhoodSherwood Forest  
##                                 2.850e+02  
##                 neighborhoodSouth Atlanta  
##                                -5.961e+01  
##           neighborhoodSouth River Gardens  
##                                -1.296e+02  
##             neighborhoodSouth Tuxedo Park  
##                                 5.620e+01  
##                     neighborhoodSouthwest  
##                                -5.395e+01  
##                    neighborhoodSpringlake  
##                                -1.311e+01  
##                    neighborhoodSummerhill  
##                                -2.700e+01  
##        neighborhoodSwallow Circle/Baywood  
##                                -9.609e+01  
##                  neighborhoodSweet Auburn  
##                                 3.076e+02  
##                  neighborhoodSylvan Hills  
##                                -1.025e+02  
##                    neighborhoodTampa Park  
##                                -1.020e+02  
##        neighborhoodThe Villages at Carver  
##                                -6.709e+01  
## neighborhoodThe Villages at Castleberry H  
##                                -2.192e+02  
##     neighborhoodThe Villages at East Lake  
##                                 3.413e+02  
##           neighborhoodThomasville Heights  
##                                -5.676e+01  
##                   neighborhoodTuxedo Park  
##                                 7.015e+01  
##               neighborhoodUnderwood Hills  
##                                -3.568e+01  
##                neighborhoodVenetian Hills  
##                                 2.570e+00  
##                     neighborhoodVine City  
##                                -2.103e+02  
##             neighborhoodVirginia Highland  
##                                 3.581e+02  
##               neighborhoodWashington Park  
##                                -2.107e+02  
##                 neighborhoodWesley Battle  
##                                -7.726e+00  
##                      neighborhoodWest End  
##                                 3.430e+01  
##                neighborhoodWest Highlands  
##                                -1.699e+02  
##                     neighborhoodWest Lake  
##                                -2.236e+02  
##                    neighborhoodWest Manor  
##                                -1.777e+00  
##    neighborhoodWest Paces Ferry/Northside  
##                                 4.484e+01  
##                     neighborhoodWesthaven  
##                                -2.468e+02  
##            neighborhoodWestminster/Milmar  
##                                 1.349e+01  
##           neighborhoodWestover Plantation  
##                                -1.964e+01  
##                      neighborhoodWestview  
##                                 4.217e+01  
##              neighborhoodWestwood Terrace  
##                                 3.232e+01  
##              neighborhoodWhitewater Creek  
##                                 6.154e+01  
##         neighborhoodWhittier Mill Village  
##                                -6.355e+01  
##              neighborhoodWildwood (NPU-C)  
##                                -2.229e+01  
##              neighborhoodWildwood (NPU-H)  
##                                 7.433e-01  
##               neighborhoodWildwood Forest  
##                                -1.140e+02  
##           neighborhoodWilson Mill Meadows  
##                                 1.149e+01  
##              neighborhoodWisteria Gardens  
##                                 2.528e+01  
##                     neighborhoodWoodfield  
##                                 1.398e+00  
##                neighborhoodWoodland Hills  
##                                 2.733e+02  
##                       neighborhoodWyngate  
##                                 2.482e+01  
##                                         x  
##                                -6.831e+02  
##                                         y  
##                                -1.708e+03
summary(fit)
## 
## Call:
## lm(formula = beat ~ MinOfucr + MaxOfnum_victims + loc_type + 
##     neighborhood + x + y, data = COBRA_YTD2017)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -529.44   -5.40    0.22    6.06  414.65 
## 
## Coefficients:
##                                             Estimate Std. Error  t value
## (Intercept)                                3.088e+02  3.606e+00   85.642
## MinOfucr                                   2.221e-02  2.590e-03    8.575
## MaxOfnum_victims                          -2.153e-01  3.911e-01   -0.550
## loc_type                                  -1.020e-01  1.629e-02   -6.258
## neighborhoodAdair Park                    -5.594e+01  3.485e+00  -16.050
## neighborhoodAdams Park                    -1.071e+01  5.388e+00   -1.988
## neighborhoodAdamsville                    -1.072e+02  3.165e+00  -33.883
## neighborhoodAlmond Park                   -1.922e+02  6.025e+00  -31.892
## neighborhoodAmal Heights                  -8.001e+01  8.495e+00   -9.419
## neighborhoodAnsley Park                    2.682e+02  5.615e+00   47.761
## neighborhoodArden/Habersham                3.451e+01  1.775e+01    1.945
## neighborhoodArdmore                       -1.492e+01  7.784e+00   -1.917
## neighborhoodArgonne Forest                 3.535e+01  1.409e+01    2.509
## neighborhoodArlington Estates             -9.771e+01  8.997e+00  -10.860
## neighborhoodAshley Courts                 -4.434e+01  6.127e+00   -7.237
## neighborhoodAshview Heights               -2.293e+02  3.854e+00  -59.512
## neighborhoodAtkins Park                    3.590e+02  2.281e+01   15.739
## neighborhoodAtlanta Industrial Park       -1.911e+02  8.154e+00  -23.430
## neighborhoodAtlanta University Center     -2.148e+02  4.086e+00  -52.588
## neighborhoodAtlantic Station               2.518e+02  3.440e+00   73.210
## neighborhoodAudobon Forest                 4.003e+00  9.935e+00    0.403
## neighborhoodAudobon Forest West           -1.011e+01  1.495e+01   -0.676
## neighborhoodBaker Hills                    2.365e+01  7.551e+00    3.132
## neighborhoodBakers Ferry                   3.247e+00  1.767e+01    0.184
## neighborhoodBankhead                      -1.978e+02  3.669e+00  -53.917
## neighborhoodBankhead/Bolton               -2.120e+02  9.131e+00  -23.220
## neighborhoodBeecher Hills                  1.716e+01  1.100e+01    1.560
## neighborhoodBen Hill                      -1.239e+02  6.952e+00  -17.828
## neighborhoodBen Hill Acres                -8.011e+01  8.061e+00   -9.938
## neighborhoodBen Hill Forest               -1.018e+02  2.793e+01   -3.644
## neighborhoodBen Hill Pines                -9.275e+01  1.619e+01   -5.730
## neighborhoodBen Hill Terrace              -7.799e+01  7.388e+00  -10.557
## neighborhoodBenteen Park                   2.536e+02  6.511e+00   38.959
## neighborhoodBerkeley Park                 -3.422e+01  2.867e+00  -11.937
## neighborhoodBetmar LaVilla                -8.306e+01  5.885e+00  -14.114
## neighborhoodBlair Villa/Poole Creek       -1.533e+02  5.487e+00  -27.930
## neighborhoodBlandtown                     -5.819e+01  3.137e+00  -18.548
## neighborhoodBolton                        -3.747e+01  4.300e+00   -8.715
## neighborhoodBolton Hills                  -1.732e+02  1.615e+01  -10.724
## neighborhoodBoulder Park                  -2.017e+00  1.614e+01   -0.125
## neighborhoodBoulevard Heights              2.708e+02  6.281e+00   43.112
## neighborhoodBrandon                       -5.773e+00  1.153e+01   -0.501
## neighborhoodBrentwood                     -1.044e+02  1.501e+01   -6.955
## neighborhoodBriar Glen                    -6.678e+01  1.617e+01   -4.130
## neighborhoodBrookhaven                     1.103e+02  1.273e+01    8.662
## neighborhoodBrookview Heights             -2.056e+02  7.693e+00  -26.727
## neighborhoodBrookwood                     -1.984e+01  7.172e+00   -2.766
## neighborhoodBrookwood Hills               -1.158e+01  6.011e+00   -1.927
## neighborhoodBrowns Mill Park              -1.083e+02  3.911e+00  -27.692
## neighborhoodBuckhead Forest                6.822e+01  3.968e+00   17.195
## neighborhoodBuckhead Heights               8.416e+01  6.912e+00   12.176
## neighborhoodBuckhead Village               5.630e+01  3.787e+00   14.866
## neighborhoodBush Mountain                  2.443e+01  8.318e+00    2.937
## neighborhoodButner/Tell                   -1.055e+02  2.283e+01   -4.622
## neighborhoodCabbagetown                    3.089e+02  5.968e+00   51.762
## neighborhoodCampbellton Road              -2.648e+01  3.154e+00   -8.394
## neighborhoodCandler Park                   3.534e+02  3.686e+00   95.885
## neighborhoodCapitol Gateway                2.862e+02  6.367e+00   44.945
## neighborhoodCapitol View                  -8.205e+01  3.850e+00  -21.314
## neighborhoodCapitol View Manor            -7.716e+01  8.688e+00   -8.880
## neighborhoodCarey Park                    -1.900e+02  4.688e+00  -40.532
## neighborhoodCarroll Heights               -2.290e+02  6.685e+00  -34.257
## neighborhoodCarver Hills                  -1.664e+02  7.586e+00  -21.934
## neighborhoodCascade Avenue/Road            1.491e+01  3.650e+00    4.084
## neighborhoodCascade Green                 -3.778e+01  1.616e+01   -2.338
## neighborhoodCascade Heights               -1.374e+01  4.688e+00   -2.932
## neighborhoodCastleberry Hill               1.847e+02  2.698e+00   68.487
## neighborhoodCastlewood                     2.267e+01  1.621e+01    1.398
## neighborhoodCenter Hill                   -2.125e+02  3.223e+00  -65.940
## neighborhoodChalet Woods                   3.127e+01  1.974e+01    1.584
## neighborhoodChanning Valley               -2.311e+01  7.515e+00   -3.075
## neighborhoodChastain Park                  9.584e+01  8.971e+00   10.684
## neighborhoodChosewood Park                -4.798e+01  4.335e+00  -11.068
## neighborhoodCollier Heights               -2.200e+02  2.884e+00  -76.292
## neighborhoodCollier Hills                 -1.576e+01  1.107e+01   -1.424
## neighborhoodCollier Hills North           -7.174e+00  2.793e+01   -0.257
## neighborhoodColonial Homes                -1.751e+00  1.772e+01   -0.099
## neighborhoodCross Creek                   -1.726e+01  9.716e+00   -1.776
## neighborhoodCuster/McDonough/Guice         2.544e+02  4.403e+00   57.784
## neighborhoodDeerwood                      -9.579e+01  9.218e+00  -10.392
## neighborhoodDixie Hills                   -2.329e+02  4.114e+00  -56.621
## neighborhoodDowntown                       2.113e+02  1.675e+00  126.187
## neighborhoodDruid Hills                    3.727e+02  7.058e+00   52.809
## neighborhoodEast Ardley Road              -1.065e+01  2.279e+01   -0.467
## neighborhoodEast Atlanta                   3.081e+02  2.643e+00  116.587
## neighborhoodEast Chastain Park             1.171e+02  7.042e+00   16.635
## neighborhoodEast Lake                      3.528e+02  3.620e+00   97.446
## neighborhoodEdgewood                       3.384e+02  2.413e+00  140.246
## neighborhoodElmco Estates                 -9.468e+01  1.001e+01   -9.454
## neighborhoodEnglish Avenue                -1.822e+02  2.800e+00  -65.058
## neighborhoodEnglish Park                  -1.962e+02  9.647e+00  -20.340
## neighborhoodFairburn                      -8.784e+01  8.072e+00  -10.882
## neighborhoodFairburn Heights              -2.392e+02  5.658e+00  -42.277
## neighborhoodFairburn Mays                 -2.608e-01  4.187e+00   -0.062
## neighborhoodFairburn Road/Wisteria Lane    1.013e+01  1.495e+01    0.677
## neighborhoodFairburn Tell                 -9.799e+01  3.945e+01   -2.484
## neighborhoodFairway Acres                 -1.051e+02  1.619e+01   -6.490
## neighborhoodFernleaf                      -2.435e+01  1.618e+01   -1.505
## neighborhoodFlorida Heights               -1.341e+02  4.629e+00  -28.959
## neighborhoodFort McPherson                -1.178e+00  2.790e+01   -0.042
## neighborhoodFort Valley                   -1.903e+01  6.628e+00   -2.871
## neighborhoodGarden Hills                   4.575e+01  4.233e+00   10.810
## neighborhoodGeorgia Tech                   2.323e+02  2.791e+01    8.325
## neighborhoodGlenrose Heights              -1.316e+02  3.279e+00  -40.127
## neighborhoodGrant Park                     2.886e+02  2.533e+00  113.945
## neighborhoodGreen Acres Valley            -1.467e+01  1.768e+01   -0.830
## neighborhoodGreen Forest Acres            -8.231e+00  1.400e+01   -0.588
## neighborhoodGreenbriar                    -8.013e+01  2.670e+00  -30.016
## neighborhoodGreenbriar Village            -7.815e+01  1.404e+01   -5.568
## neighborhoodGrove Park                    -2.037e+02  2.677e+00  -76.086
## neighborhoodHammond Park                  -1.334e+02  3.487e+00  -38.266
## neighborhoodHanover West                  -1.605e+01  1.771e+01   -0.906
## neighborhoodHarland Terrace                3.398e+00  3.053e+00    1.113
## neighborhoodHarris Chiles                 -2.319e+02  5.321e+00  -43.571
## neighborhoodHarvel Homes Community        -2.362e+02  2.790e+01   -8.467
## neighborhoodHeritage Valley               -5.375e+01  9.971e+00   -5.391
## neighborhoodHigh Point                    -6.895e+01  9.639e+00   -7.153
## neighborhoodHills Park                    -5.258e+01  4.836e+00  -10.874
## neighborhoodHome Park                      2.364e+02  2.476e+00   95.492
## neighborhoodHorseshoe Community           -2.681e+01  2.790e+01   -0.961
## neighborhoodHunter Hills                  -2.145e+02  3.777e+00  -56.789
## neighborhoodHuntington                    -1.373e+02  2.285e+01   -6.008
## neighborhoodInman Park                     3.329e+02  2.612e+00  127.487
## neighborhoodIvan Hill                      1.963e+01  1.252e+01    1.567
## neighborhoodJoyland                       -7.231e+01  7.081e+00  -10.211
## neighborhoodJust Us                       -2.265e+02  3.943e+01   -5.744
## neighborhoodKings Forest                  -7.272e+01  5.710e+00  -12.736
## neighborhoodKingswood                      4.664e+01  2.795e+01    1.668
## neighborhoodKirkwood                       3.499e+02  3.518e+00   99.454
## neighborhoodKnight Park/Howell Station    -1.739e+02  7.008e+00  -24.816
## neighborhoodLake Claire                    3.634e+02  5.913e+00   61.455
## neighborhoodLake Estates                  -1.091e+02  3.947e+01   -2.764
## neighborhoodLakewood                      -8.625e+01  6.602e+00  -13.064
## neighborhoodLakewood Heights              -7.485e+01  2.594e+00  -28.855
## neighborhoodLaurens Valley                -3.553e+01  2.790e+01   -1.273
## neighborhoodLeila Valley                  -8.211e+01  6.008e+00  -13.666
## neighborhoodLenox                          8.408e+01  3.095e+00   27.168
## neighborhoodLincoln Homes                 -1.737e+02  8.004e+00  -21.695
## neighborhoodLindbergh/Morosgo              4.112e+01  2.714e+00   15.147
## neighborhoodLindridge/Martin Manor         4.212e+01  3.498e+00   12.041
## neighborhoodLoring Heights                -3.741e+01  3.625e+00  -10.321
## neighborhoodMagnum Manor                  -1.892e+01  1.321e+01   -1.433
## neighborhoodMargaret Mitchell             -3.024e+00  1.620e+01   -0.187
## neighborhoodMarietta Street Artery         2.257e+02  3.741e+00   60.326
## neighborhoodMays                           8.219e+00  5.288e+00    1.554
## neighborhoodMeadowbrook Forest            -7.720e+01  1.200e+01   -6.434
## neighborhoodMechanicsville                -3.398e+01  2.337e+00  -14.540
## neighborhoodMellwood                      -2.445e+02  2.792e+01   -8.758
## neighborhoodMemorial Park                 -4.356e+00  2.793e+01   -0.156
## neighborhoodMidtown                        2.483e+02  1.929e+00  128.724
## neighborhoodMidwest Cascade               -3.968e+01  6.914e+00   -5.739
## neighborhoodMonroe Heights                -1.826e+02  6.535e+00  -27.947
## neighborhoodMorningside/Lenox Park         1.177e+01  2.927e+00    4.023
## neighborhoodMozley Park                   -2.374e+02  4.292e+00  -55.304
## neighborhoodMt. Gilead Woods              -6.112e+01  1.498e+01   -4.080
## neighborhoodMt. Paran Parkway              8.077e+01  3.949e+01    2.046
## neighborhoodMt. Paran/Northside            7.260e+01  9.851e+00    7.370
## neighborhoodNiskey Cove                   -6.749e+01  2.791e+01   -2.418
## neighborhoodNiskey Lake                   -5.861e+01  1.977e+01   -2.965
## neighborhoodNorth Buckhead                 8.552e+01  3.131e+00   27.315
## neighborhoodNorwood Manor                 -7.743e+01  6.433e+00  -12.036
## neighborhoodOakcliff                      -2.579e+02  1.495e+01  -17.252
## neighborhoodOakland                        2.976e+02  8.700e+00   34.202
## neighborhoodOakland City                   1.604e+01  2.971e+00    5.398
## neighborhoodOld Fairburn Village          -4.514e+01  3.944e+01   -1.145
## neighborhoodOld Fourth Ward                3.293e+02  2.142e+00  153.720
## neighborhoodOld Gordon                    -2.503e+02  8.688e+00  -28.811
## neighborhoodOrchard Knob                  -1.258e+02  6.902e+00  -18.222
## neighborhoodOrmewood Park                  2.985e+02  3.582e+00   83.338
## neighborhoodPaces                          2.661e+01  8.001e+00    3.326
## neighborhoodPeachtree Battle Alliance      6.536e+00  1.109e+01    0.589
## neighborhoodPeachtree Heights East         3.028e+01  1.112e+01    2.722
## neighborhoodPeachtree Heights West         4.196e+01  4.710e+00    8.907
## neighborhoodPeachtree Hills                2.127e+01  5.958e+00    3.570
## neighborhoodPeachtree Park                 6.859e+01  5.270e+00   13.015
## neighborhoodPenelope Neighbors            -2.376e+02  1.145e+01  -20.752
## neighborhoodPeoplestown                   -4.417e+01  3.498e+00  -12.628
## neighborhoodPerkerson                     -1.263e+02  3.072e+00  -41.123
## neighborhoodPeyton Forest                  2.126e+01  1.252e+01    1.698
## neighborhoodPiedmont Heights               7.225e+00  3.350e+00    2.157
## neighborhoodPine Hills                     6.717e+01  4.197e+00   16.003
## neighborhoodPittsburgh                    -5.171e+01  2.691e+00  -19.219
## neighborhoodPleasant Hill                  3.884e+01  1.776e+01    2.188
## neighborhoodPolar Rock                    -9.784e+01  7.311e+00  -13.382
## neighborhoodPomona Park                   -1.539e+01  2.791e+01   -0.551
## neighborhoodPoncey-Highland                3.494e+02  3.385e+00  103.210
## neighborhoodPrinceton Lakes               -1.367e+02  2.929e+00  -46.686
## neighborhoodRandall Mill                   3.644e+01  7.389e+00    4.931
## neighborhoodRebel Valley Forest           -8.822e+01  6.979e+00  -12.641
## neighborhoodReynoldstown                   3.229e+02  4.095e+00   78.851
## neighborhoodRidgecrest Forest             -3.635e-01  1.196e+01   -0.030
## neighborhoodRidgedale Park                 9.914e+01  7.993e+00   12.404
## neighborhoodRidgewood Heights             -2.189e+01  1.151e+01   -1.902
## neighborhoodRiverside                     -6.176e+01  4.312e+00  -14.322
## neighborhoodRockdale                      -1.745e+02  5.976e+00  -29.196
## neighborhoodRosedale Heights              -1.086e+02  6.531e+00  -16.628
## neighborhoodRue Royal                     -8.399e+01  2.793e+01   -3.007
## neighborhoodSandlewood Estates            -8.392e+01  1.151e+01   -7.291
## neighborhoodScotts Crossing               -1.555e+02  5.760e+00  -26.992
## neighborhoodSherwood Forest                2.850e+02  1.978e+01   14.403
## neighborhoodSouth Atlanta                 -5.961e+01  4.040e+00  -14.754
## neighborhoodSouth River Gardens           -1.296e+02  3.460e+00  -37.469
## neighborhoodSouth Tuxedo Park              5.620e+01  4.466e+00   12.584
## neighborhoodSouthwest                     -5.395e+01  3.591e+00  -15.023
## neighborhoodSpringlake                    -1.311e+01  1.068e+01   -1.228
## neighborhoodSummerhill                    -2.700e+01  3.653e+00   -7.390
## neighborhoodSwallow Circle/Baywood        -9.609e+01  1.195e+01   -8.040
## neighborhoodSweet Auburn                   3.076e+02  3.034e+00  101.394
## neighborhoodSylvan Hills                  -1.025e+02  2.529e+00  -40.503
## neighborhoodTampa Park                    -1.020e+02  1.979e+01   -5.152
## neighborhoodThe Villages at Carver        -6.709e+01  4.093e+00  -16.392
## neighborhoodThe Villages at Castleberry H -2.192e+02  5.609e+00  -39.073
## neighborhoodThe Villages at East Lake      3.413e+02  5.933e+00   57.521
## neighborhoodThomasville Heights           -5.676e+01  4.229e+00  -13.421
## neighborhoodTuxedo Park                    7.015e+01  1.079e+01    6.498
## neighborhoodUnderwood Hills               -3.568e+01  3.001e+00  -11.891
## neighborhoodVenetian Hills                 2.570e+00  3.034e+00    0.847
## neighborhoodVine City                     -2.103e+02  2.562e+00  -82.070
## neighborhoodVirginia Highland              3.581e+02  3.043e+00  117.673
## neighborhoodWashington Park               -2.107e+02  4.591e+00  -45.891
## neighborhoodWesley Battle                 -7.726e+00  1.501e+01   -0.515
## neighborhoodWest End                       3.430e+01  2.195e+00   15.628
## neighborhoodWest Highlands                -1.699e+02  4.480e+00  -37.937
## neighborhoodWest Lake                     -2.236e+02  5.716e+00  -39.116
## neighborhoodWest Manor                    -1.777e+00  8.697e+00   -0.204
## neighborhoodWest Paces Ferry/Northside     4.484e+01  7.275e+00    6.164
## neighborhoodWesthaven                     -2.468e+02  8.898e+00  -27.732
## neighborhoodWestminster/Milmar             1.349e+01  1.621e+01    0.832
## neighborhoodWestover Plantation           -1.964e+01  2.282e+01   -0.861
## neighborhoodWestview                       4.217e+01  3.296e+00   12.794
## neighborhoodWestwood Terrace               3.232e+01  8.310e+00    3.889
## neighborhoodWhitewater Creek               6.154e+01  1.625e+01    3.786
## neighborhoodWhittier Mill Village         -6.355e+01  5.825e+00  -10.910
## neighborhoodWildwood (NPU-C)              -2.229e+01  4.832e+00   -4.613
## neighborhoodWildwood (NPU-H)               7.433e-01  8.698e+00    0.085
## neighborhoodWildwood Forest               -1.140e+02  1.980e+01   -5.761
## neighborhoodWilson Mill Meadows            1.149e+01  7.427e+00    1.547
## neighborhoodWisteria Gardens               2.528e+01  1.061e+01    2.383
## neighborhoodWoodfield                      1.398e+00  1.979e+01    0.071
## neighborhoodWoodland Hills                 2.733e+02  5.824e+00   46.915
## neighborhoodWyngate                        2.482e+01  1.503e+01    1.651
## x                                         -6.831e+02  6.001e+00 -113.828
## y                                         -1.708e+03  1.501e+01 -113.839
##                                           Pr(>|t|)    
## (Intercept)                                < 2e-16 ***
## MinOfucr                                   < 2e-16 ***
## MaxOfnum_victims                          0.582092    
## loc_type                                  3.97e-10 ***
## neighborhoodAdair Park                     < 2e-16 ***
## neighborhoodAdams Park                    0.046818 *  
## neighborhoodAdamsville                     < 2e-16 ***
## neighborhoodAlmond Park                    < 2e-16 ***
## neighborhoodAmal Heights                   < 2e-16 ***
## neighborhoodAnsley Park                    < 2e-16 ***
## neighborhoodArden/Habersham               0.051814 .  
## neighborhoodArdmore                       0.055272 .  
## neighborhoodArgonne Forest                0.012120 *  
## neighborhoodArlington Estates              < 2e-16 ***
## neighborhoodAshley Courts                 4.73e-13 ***
## neighborhoodAshview Heights                < 2e-16 ***
## neighborhoodAtkins Park                    < 2e-16 ***
## neighborhoodAtlanta Industrial Park        < 2e-16 ***
## neighborhoodAtlanta University Center      < 2e-16 ***
## neighborhoodAtlantic Station               < 2e-16 ***
## neighborhoodAudobon Forest                0.687009    
## neighborhoodAudobon Forest West           0.498808    
## neighborhoodBaker Hills                   0.001737 ** 
## neighborhoodBakers Ferry                  0.854218    
## neighborhoodBankhead                       < 2e-16 ***
## neighborhoodBankhead/Bolton                < 2e-16 ***
## neighborhoodBeecher Hills                 0.118877    
## neighborhoodBen Hill                       < 2e-16 ***
## neighborhoodBen Hill Acres                 < 2e-16 ***
## neighborhoodBen Hill Forest               0.000269 ***
## neighborhoodBen Hill Pines                1.02e-08 ***
## neighborhoodBen Hill Terrace               < 2e-16 ***
## neighborhoodBenteen Park                   < 2e-16 ***
## neighborhoodBerkeley Park                  < 2e-16 ***
## neighborhoodBetmar LaVilla                 < 2e-16 ***
## neighborhoodBlair Villa/Poole Creek        < 2e-16 ***
## neighborhoodBlandtown                      < 2e-16 ***
## neighborhoodBolton                         < 2e-16 ***
## neighborhoodBolton Hills                   < 2e-16 ***
## neighborhoodBoulder Park                  0.900583    
## neighborhoodBoulevard Heights              < 2e-16 ***
## neighborhoodBrandon                       0.616518    
## neighborhoodBrentwood                     3.62e-12 ***
## neighborhoodBriar Glen                    3.64e-05 ***
## neighborhoodBrookhaven                     < 2e-16 ***
## neighborhoodBrookview Heights              < 2e-16 ***
## neighborhoodBrookwood                     0.005674 ** 
## neighborhoodBrookwood Hills               0.054002 .  
## neighborhoodBrowns Mill Park               < 2e-16 ***
## neighborhoodBuckhead Forest                < 2e-16 ***
## neighborhoodBuckhead Heights               < 2e-16 ***
## neighborhoodBuckhead Village               < 2e-16 ***
## neighborhoodBush Mountain                 0.003316 ** 
## neighborhoodButner/Tell                   3.82e-06 ***
## neighborhoodCabbagetown                    < 2e-16 ***
## neighborhoodCampbellton Road               < 2e-16 ***
## neighborhoodCandler Park                   < 2e-16 ***
## neighborhoodCapitol Gateway                < 2e-16 ***
## neighborhoodCapitol View                   < 2e-16 ***
## neighborhoodCapitol View Manor             < 2e-16 ***
## neighborhoodCarey Park                     < 2e-16 ***
## neighborhoodCarroll Heights                < 2e-16 ***
## neighborhoodCarver Hills                   < 2e-16 ***
## neighborhoodCascade Avenue/Road           4.45e-05 ***
## neighborhoodCascade Green                 0.019380 *  
## neighborhoodCascade Heights               0.003374 ** 
## neighborhoodCastleberry Hill               < 2e-16 ***
## neighborhoodCastlewood                    0.162018    
## neighborhoodCenter Hill                    < 2e-16 ***
## neighborhoodChalet Woods                  0.113263    
## neighborhoodChanning Valley               0.002108 ** 
## neighborhoodChastain Park                  < 2e-16 ***
## neighborhoodChosewood Park                 < 2e-16 ***
## neighborhoodCollier Heights                < 2e-16 ***
## neighborhoodCollier Hills                 0.154439    
## neighborhoodCollier Hills North           0.797261    
## neighborhoodColonial Homes                0.921295    
## neighborhoodCross Creek                   0.075671 .  
## neighborhoodCuster/McDonough/Guice         < 2e-16 ***
## neighborhoodDeerwood                       < 2e-16 ***
## neighborhoodDixie Hills                    < 2e-16 ***
## neighborhoodDowntown                       < 2e-16 ***
## neighborhoodDruid Hills                    < 2e-16 ***
## neighborhoodEast Ardley Road              0.640317    
## neighborhoodEast Atlanta                   < 2e-16 ***
## neighborhoodEast Chastain Park             < 2e-16 ***
## neighborhoodEast Lake                      < 2e-16 ***
## neighborhoodEdgewood                       < 2e-16 ***
## neighborhoodElmco Estates                  < 2e-16 ***
## neighborhoodEnglish Avenue                 < 2e-16 ***
## neighborhoodEnglish Park                   < 2e-16 ***
## neighborhoodFairburn                       < 2e-16 ***
## neighborhoodFairburn Heights               < 2e-16 ***
## neighborhoodFairburn Mays                 0.950328    
## neighborhoodFairburn Road/Wisteria Lane   0.498121    
## neighborhoodFairburn Tell                 0.013008 *  
## neighborhoodFairway Acres                 8.76e-11 ***
## neighborhoodFernleaf                      0.132436    
## neighborhoodFlorida Heights                < 2e-16 ***
## neighborhoodFort McPherson                0.966315    
## neighborhoodFort Valley                   0.004089 ** 
## neighborhoodGarden Hills                   < 2e-16 ***
## neighborhoodGeorgia Tech                   < 2e-16 ***
## neighborhoodGlenrose Heights               < 2e-16 ***
## neighborhoodGrant Park                     < 2e-16 ***
## neighborhoodGreen Acres Valley            0.406707    
## neighborhoodGreen Forest Acres            0.556440    
## neighborhoodGreenbriar                     < 2e-16 ***
## neighborhoodGreenbriar Village            2.61e-08 ***
## neighborhoodGrove Park                     < 2e-16 ***
## neighborhoodHammond Park                   < 2e-16 ***
## neighborhoodHanover West                  0.364883    
## neighborhoodHarland Terrace               0.265752    
## neighborhoodHarris Chiles                  < 2e-16 ***
## neighborhoodHarvel Homes Community         < 2e-16 ***
## neighborhoodHeritage Valley               7.09e-08 ***
## neighborhoodHigh Point                    8.75e-13 ***
## neighborhoodHills Park                     < 2e-16 ***
## neighborhoodHome Park                      < 2e-16 ***
## neighborhoodHorseshoe Community           0.336607    
## neighborhoodHunter Hills                   < 2e-16 ***
## neighborhoodHuntington                    1.91e-09 ***
## neighborhoodInman Park                     < 2e-16 ***
## neighborhoodIvan Hill                     0.117151    
## neighborhoodJoyland                        < 2e-16 ***
## neighborhoodJust Us                       9.39e-09 ***
## neighborhoodKings Forest                   < 2e-16 ***
## neighborhoodKingswood                     0.095246 .  
## neighborhoodKirkwood                       < 2e-16 ***
## neighborhoodKnight Park/Howell Station     < 2e-16 ***
## neighborhoodLake Claire                    < 2e-16 ***
## neighborhoodLake Estates                  0.005716 ** 
## neighborhoodLakewood                       < 2e-16 ***
## neighborhoodLakewood Heights               < 2e-16 ***
## neighborhoodLaurens Valley                0.202956    
## neighborhoodLeila Valley                   < 2e-16 ***
## neighborhoodLenox                          < 2e-16 ***
## neighborhoodLincoln Homes                  < 2e-16 ***
## neighborhoodLindbergh/Morosgo              < 2e-16 ***
## neighborhoodLindridge/Martin Manor         < 2e-16 ***
## neighborhoodLoring Heights                 < 2e-16 ***
## neighborhoodMagnum Manor                  0.152003    
## neighborhoodMargaret Mitchell             0.851911    
## neighborhoodMarietta Street Artery         < 2e-16 ***
## neighborhoodMays                          0.120166    
## neighborhoodMeadowbrook Forest            1.27e-10 ***
## neighborhoodMechanicsville                 < 2e-16 ***
## neighborhoodMellwood                       < 2e-16 ***
## neighborhoodMemorial Park                 0.876069    
## neighborhoodMidtown                        < 2e-16 ***
## neighborhoodMidwest Cascade               9.66e-09 ***
## neighborhoodMonroe Heights                 < 2e-16 ***
## neighborhoodMorningside/Lenox Park        5.77e-05 ***
## neighborhoodMozley Park                    < 2e-16 ***
## neighborhoodMt. Gilead Woods              4.52e-05 ***
## neighborhoodMt. Paran Parkway             0.040809 *  
## neighborhoodMt. Paran/Northside           1.77e-13 ***
## neighborhoodNiskey Cove                   0.015619 *  
## neighborhoodNiskey Lake                   0.003028 ** 
## neighborhoodNorth Buckhead                 < 2e-16 ***
## neighborhoodNorwood Manor                  < 2e-16 ***
## neighborhoodOakcliff                       < 2e-16 ***
## neighborhoodOakland                        < 2e-16 ***
## neighborhoodOakland City                  6.80e-08 ***
## neighborhoodOld Fairburn Village          0.252421    
## neighborhoodOld Fourth Ward                < 2e-16 ***
## neighborhoodOld Gordon                     < 2e-16 ***
## neighborhoodOrchard Knob                   < 2e-16 ***
## neighborhoodOrmewood Park                  < 2e-16 ***
## neighborhoodPaces                         0.000882 ***
## neighborhoodPeachtree Battle Alliance     0.555800    
## neighborhoodPeachtree Heights East        0.006487 ** 
## neighborhoodPeachtree Heights West         < 2e-16 ***
## neighborhoodPeachtree Hills               0.000358 ***
## neighborhoodPeachtree Park                 < 2e-16 ***
## neighborhoodPenelope Neighbors             < 2e-16 ***
## neighborhoodPeoplestown                    < 2e-16 ***
## neighborhoodPerkerson                      < 2e-16 ***
## neighborhoodPeyton Forest                 0.089554 .  
## neighborhoodPiedmont Heights              0.031022 *  
## neighborhoodPine Hills                     < 2e-16 ***
## neighborhoodPittsburgh                     < 2e-16 ***
## neighborhoodPleasant Hill                 0.028707 *  
## neighborhoodPolar Rock                     < 2e-16 ***
## neighborhoodPomona Park                   0.581376    
## neighborhoodPoncey-Highland                < 2e-16 ***
## neighborhoodPrinceton Lakes                < 2e-16 ***
## neighborhoodRandall Mill                  8.23e-07 ***
## neighborhoodRebel Valley Forest            < 2e-16 ***
## neighborhoodReynoldstown                   < 2e-16 ***
## neighborhoodRidgecrest Forest             0.975746    
## neighborhoodRidgedale Park                 < 2e-16 ***
## neighborhoodRidgewood Heights             0.057201 .  
## neighborhoodRiverside                      < 2e-16 ***
## neighborhoodRockdale                       < 2e-16 ***
## neighborhoodRosedale Heights               < 2e-16 ***
## neighborhoodRue Royal                     0.002637 ** 
## neighborhoodSandlewood Estates            3.18e-13 ***
## neighborhoodScotts Crossing                < 2e-16 ***
## neighborhoodSherwood Forest                < 2e-16 ***
## neighborhoodSouth Atlanta                  < 2e-16 ***
## neighborhoodSouth River Gardens            < 2e-16 ***
## neighborhoodSouth Tuxedo Park              < 2e-16 ***
## neighborhoodSouthwest                      < 2e-16 ***
## neighborhoodSpringlake                    0.219580    
## neighborhoodSummerhill                    1.51e-13 ***
## neighborhoodSwallow Circle/Baywood        9.42e-16 ***
## neighborhoodSweet Auburn                   < 2e-16 ***
## neighborhoodSylvan Hills                   < 2e-16 ***
## neighborhoodTampa Park                    2.60e-07 ***
## neighborhoodThe Villages at Carver         < 2e-16 ***
## neighborhoodThe Villages at Castleberry H  < 2e-16 ***
## neighborhoodThe Villages at East Lake      < 2e-16 ***
## neighborhoodThomasville Heights            < 2e-16 ***
## neighborhoodTuxedo Park                   8.30e-11 ***
## neighborhoodUnderwood Hills                < 2e-16 ***
## neighborhoodVenetian Hills                0.397000    
## neighborhoodVine City                      < 2e-16 ***
## neighborhoodVirginia Highland              < 2e-16 ***
## neighborhoodWashington Park                < 2e-16 ***
## neighborhoodWesley Battle                 0.606691    
## neighborhoodWest End                       < 2e-16 ***
## neighborhoodWest Highlands                 < 2e-16 ***
## neighborhoodWest Lake                      < 2e-16 ***
## neighborhoodWest Manor                    0.838127    
## neighborhoodWest Paces Ferry/Northside    7.23e-10 ***
## neighborhoodWesthaven                      < 2e-16 ***
## neighborhoodWestminster/Milmar            0.405328    
## neighborhoodWestover Plantation           0.389368    
## neighborhoodWestview                       < 2e-16 ***
## neighborhoodWestwood Terrace              0.000101 ***
## neighborhoodWhitewater Creek              0.000153 ***
## neighborhoodWhittier Mill Village          < 2e-16 ***
## neighborhoodWildwood (NPU-C)              3.98e-06 ***
## neighborhoodWildwood (NPU-H)              0.931892    
## neighborhoodWildwood Forest               8.47e-09 ***
## neighborhoodWilson Mill Meadows           0.121980    
## neighborhoodWisteria Gardens              0.017168 *  
## neighborhoodWoodfield                     0.943692    
## neighborhoodWoodland Hills                 < 2e-16 ***
## neighborhoodWyngate                       0.098731 .  
## x                                          < 2e-16 ***
## y                                          < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 39.41 on 23172 degrees of freedom
##   (3344 observations deleted due to missingness)
## Multiple R-squared:  0.9464, Adjusted R-squared:  0.9459 
## F-statistic:  1692 on 242 and 23172 DF,  p-value: < 2.2e-16
vif(fit)
##                          GVIF  Df GVIF^(1/(2*Df))
## MinOfucr             1.171762   1        1.082480
## MaxOfnum_victims     1.024168   1        1.012012
## loc_type             1.026056   1        1.012944
## neighborhood        11.793507 237        1.005219
## x                23807.337630   1      154.296266
## y                23818.989658   1      154.334020

? P-values are very important because, we can consider linear model to be statistically significant only.
? When both these p-values are less that the pre-determined statistical determined level, which is ideally 0.05.
? This is visually interpreted by the significance stars at the end of the row.
? The more stars beside the variable’s p-value, the more significant the variable.

When there is a p-value, there is a null and alternative hypothesis associated with it.
Null and Alternate Hypothesis
? In Linear regression, the Null Hypothesis is that the coefficients associated with the variables is equal to zero.
? The alternate hypothesis is that the coefficients are not equal to zero.
? There exists a relationship between the independent variable in question and the dependent variable.

                         GVIF  Df GVIF^(1/(2*Df))
beat                18.615002   1        4.314511
MaxOfnum_victims     1.016947   1        1.008438
loc_type             1.027692   1        1.013752
neighborhood       201.207857 237        1.011253
x                37103.058286   1      192.621542
y                37121.328278   1      192.668960




                                     
Coefficient,Estimate, std.error,Tval         
                                                &Pr(>|t|)               


neighborhoodIvan Hill
0.18995
31.7182
0.006
0.995222
neighborhoodPomona Park
-0.70578
70.67374
-0.01
0.992032
neighborhoodEast Ardley Road
0.81515
57.71572
0.014
0.988731
neighborhoodOrchard Knob
0.78361
17.60243
0.045
0.964492
neighborhoodCascade Heights
-0.54782
11.8724
-0.046
0.963197
neighborhoodCollier Hills
-1.57088
28.03153
-0.056
0.955311
neighborhoodGreen Forest Acres
2.03632
35.43986
0.057
0.954181
neighborhoodAmal Heights
-1.74562
21.55404
-0.081
0.935452
neighborhoodGlenrose Heights
0.69584
8.58791
0.081
0.935423
neighborhoodScotts Crossing
-1.25164
14.81232
-0.084
0.93266
neighborhoodBrookwood
2.04586
18.16387
0.113
0.910322
neighborhoodBuckhead Heights
1.97872
17.55909
0.113
0.910278
neighborhoodSandlewood Estates
-3.36595
29.18066
-0.115
0.90817
neighborhoodHeritage Valley
2.94675
25.26591
0.117
0.907155
neighborhoodMt. Paran/Northside
-3.01141
24.97503
-0.121
0.904027
neighborhoodPolar Rock
2.54844
18.58454
0.137
0.890932
neighborhoodArden/Habersham
6.64751
44.94562
0.148
0.882422
neighborhoodMozley Park
-1.87526
11.56441
-0.162
0.871183
neighborhoodRidgedale Park
3.38582
20.30795
0.167
0.867589
neighborhoodFairburn
-4.33549
20.49293
-0.212
0.832452
neighborhoodNiskey Cove
15.94718
70.69396
0.226
0.82153
neighborhoodAudobon Forest
-5.97875
25.15904
-0.238
0.812164
neighborhoodMeadowbrook Forest
-8.17084
30.40992
-0.269
0.788171
neighborhoodChosewood Park
-3.06345
11.00551
-0.278
0.780741
neighborhoodChastain Park
6.54934
22.77241
0.288
0.773656
neighborhoodGreenbriar
1.99187
6.8906
0.289
0.77253
neighborhoodCascade Green
12.02139
40.91864
0.294
0.768923
neighborhoodPeyton Forest
9.89081
31.7177
0.312
0.755166
neighborhoodMt. Paran Parkway
31.53809
100.0009
0.315
0.752477
neighborhoodMonroe Heights
5.36775
16.82558
0.319
0.749712
neighborhoodMarietta Street Artery
-3.3602
10.18978
-0.33
0.741583
neighborhoodAlmond Park
5.15041
15.58872
0.33
0.741106
neighborhoodOld Fairburn Village
33.39344
99.87213
0.334
0.738109
neighborhoodEnglish Park
-8.81457
24.64719
-0.358
0.720624
neighborhoodBeecher Hills
-10.1116
27.86253
-0.363
0.716677
neighborhoodFairburn Tell
37.84429
99.91444
0.379
0.704864
neighborhoodFairway Acres
15.97263
41.04548
0.389
0.697173
neighborhoodHorseshoe Community
-27.5546
70.65781
-0.39
0.696561
neighborhoodBrookhaven
13.62287
32.29803
0.422
0.673185
neighborhoodHuntington
24.97747
57.91696
0.431
0.666281
neighborhoodCabbagetown
-7.00131
15.96335
-0.439
0.660965
neighborhoodLake Claire
-7.20788
16.14665
-0.446
0.655312
neighborhoodDeerwood
11.11434
23.39644
0.475
0.63476
neighborhoodOld Gordon
10.85688
22.39042
0.485
0.627759
neighborhoodBen Hill Pines
-19.9293
41.01709
-0.486
0.627059
neighborhoodDixie Hills
-5.46251
11.11481
-0.491
0.623104
neighborhoodBakers Ferry
-22.2155
44.75458
-0.496
0.619627
neighborhoodGeorgia Tech
37.27899
70.77658
0.527
0.598397
neighborhoodRebel Valley Forest
-9.37548
17.73253
-0.529
0.597007
neighborhoodPeachtree Park
7.34794
13.39376
0.549
0.583279
neighborhoodFort McPherson
-40.5911
70.64509
-0.575
0.565583
neighborhoodPittsburgh
-4.17695
6.86732
-0.608
0.543037
neighborhoodCapitol View
-6.00118
9.84386
-0.61
0.542108
neighborhoodLindridge/Martin Manor
5.64553
8.88628
0.635
0.525233
neighborhoodBolton Hills
26.19894
41.00062
0.639
0.522837
neighborhoodAshview Heights
-6.7408
10.47775
-0.643
0.520007
neighborhoodPine Hills
6.89367
10.68744
0.645
0.518917
neighborhoodAtkins Park
-39.4179
58.07207
-0.679
0.497287
neighborhoodFairburn Road/Wisteria Lane
27.48272
37.85613
0.726
0.46786
neighborhoodBen Hill Terrace
-13.6516
18.75246
-0.728
0.466629
neighborhoodEast Chastain Park
13.9809
17.93875
0.779
0.435771
neighborhoodBrookwood Hills
12.08225
15.22187
0.794
0.427353
neighborhoodLaurens Valley
57.13108
70.6623
0.809
0.418806
neighborhoodButner/Tell
-46.879
57.83658
-0.811
0.417637
neighborhoodBriar Glen
33.76307
40.9596
0.824
0.409777
neighborhoodNorwood Manor
-13.8523
16.34179
-0.848
0.396636
neighborhoodBush Mountain
-18.4889
21.06651
-0.878
0.380147
neighborhoodCollier Hills North
64.48398
70.71828
0.912
0.361861
neighborhoodBoulder Park
-37.335
40.8767
-0.913
0.361064
neighborhoodCapitol Gateway
16.08137
16.81071
0.957
0.338772
neighborhoodMidtown
-6.27314
6.39659
-0.981
0.326751
neighborhoodGreen Acres Valley
-44.0883
44.76003
-0.985
0.324638
neighborhoodKingswood
69.87008
70.79024
0.987
0.323652
neighborhoodRidgewood Heights
29.60442
29.1443
1.016
0.309741
neighborhoodBen Hill
-18.325
17.72395
-1.034
0.301189
neighborhoodNiskey Lake
51.94337
50.06304
1.038
0.299486
neighborhoodPleasant Hill
46.81742
44.96644
1.041
0.297811
neighborhoodMellwood
74.03428
70.81976
1.045
0.295854
neighborhoodRidgecrest Forest
-31.9837
30.27555
-1.056
0.290787
neighborhoodCarroll Heights
-19.4876
17.35058
-1.123
0.261378
neighborhoodPeachtree Heights East
31.94805
28.1669
1.134
0.256705
neighborhoodPiedmont Heights
9.70736
8.48279
1.144
0.252487
neighborhoodColonial Homes
51.51639
44.87369
1.148
0.250968
neighborhoodMechanicsville
-6.86339
5.94492
-1.154
0.248308
neighborhoodAudobon Forest West
43.72563
37.86685
1.155
0.248217
neighborhoodJust Us
115.469
99.91957
1.156
0.247849
neighborhoodHunter Hills
-11.8921
10.2068
-1.165
0.243985
neighborhoodMagnum Manor
39.36476
33.44073
1.177
0.239148
neighborhoodEnglish Avenue
-9.10065
7.71194
-1.18
0.237983
neighborhoodCastlewood
48.53463
41.06086
1.182
0.237211
neighborhoodGrove Park
-9.03229
7.57827
-1.192
0.233326
neighborhoodSherwood Forest
-61.4391
50.32317
-1.221
0.22214
neighborhoodLincoln Homes
-25.341
20.47369
-1.238
0.215827
neighborhoodChalet Woods
-62.9557
49.99941
-1.259
0.207997
neighborhoodMays
-16.939
13.39209
-1.265
0.205937
neighborhoodAtlantic Station
-12.9455
9.665
-1.339
0.180446
neighborhoodMargaret Mitchell
57.26396
41.01351
1.396
0.162661
neighborhoodMemorial Park
99.55969
70.72029
1.408
0.159205
neighborhoodHome Park
-10.4807
7.4012
-1.416
0.156767
neighborhoodRosedale Heights
-23.7742
16.63523
-1.429
0.152975
neighborhoodLoring Heights
13.41215
9.19966
1.458
0.144883
neighborhoodBuckhead Village
14.2879
9.63622
1.483
0.13816
neighborhoodPerkerson
12.11922
8.05798
1.504
0.132595
neighborhoodHammond Park
-13.7955
9.10457
-1.515
0.129728
neighborhoodBen Hill Forest
108.5586
70.73677
1.535
0.124875
neighborhoodBen Hill Acres
-31.6404
20.45652
-1.547
0.121946
neighborhoodAshley Courts
-24.0345
15.53165
-1.547
0.121768
neighborhoodHanover West
69.5929
44.85392
1.552
0.120785
neighborhoodLindbergh/Morosgo
10.80412
6.9074
1.564
0.117799
neighborhoodFlorida Heights
-18.9446
11.93221
-1.588
0.112371
neighborhoodMt. Gilead Woods
-61.4997
37.94528
-1.621
0.105086
neighborhoodOakcliff
62.09365
38.09146
1.63
0.10309
lm(formula = MinOfucr ~ beat + MaxOfnum_victims + loc_type + 
    neighborhood + x + y, data = COBRA_YTD2017)

Coefficients:
                              (Intercept)                                       beat  
                                183.98780                                    0.14243

Call:
lm(formula = MinOfucr ~ beat + MaxOfnum_victims + loc_type + 
    neighborhood + x + y, data = COBRA_YTD2017)

Residuals:
    Min      1Q  Median      3Q     Max 
-333.96  -36.06   21.83   61.68  438.61 




vif(fi)>5
                  GVIF    Df GVIF^(1/(2*Df))
beat              TRUE FALSE           FALSE
MaxOfnum_victims FALSE FALSE           FALSE
loc_type          TRUE  TRUE           FALSE
neighborhood      TRUE  TRUE           FALSE
x                 TRUE FALSE            TRUE
y                 TRUE FALSE            TRUE

#b. Find out top 3 reasons for having more crime in a city.
 Which all attributes have correlation with crime rate? 


at$dayofWeek <- weekdays(as.Date(at$occur_date))
at$hour <- sub(":.*", "", at$occur_time)
at$hour <- as.numeric(at$hour)
ggplot(aes(x = hour), data = at) + geom_histogram(bins = 24, color='white', fill='black') +
  ggtitle('Histogram of Crime Time') 
#The crime time distribution appears bimodal with peaking around midnight and again at the noon, then again between 6pm and 8pm.  

#topCrimes_1 <- topCrimes %>% group_by(`UC2 Literal`,occur_time) %>% 
  #summarise(total = n())
#ggplot(aes(x = occur_time, y = total), data = topCrimes_1) +
  #geom_point(colour="blue", size=1) +
  #geom_smooth(method="loess") +
  #xlab('Hour(24 hour clock)') +
 # ylab('Number of Crimes') +
  #ggtitle('Top Crimes Time of the Day') +
  #facet_wrap(~`UC2 Literal`)

#Downtown and midtown are the most common locations where crimes take place, followed by Old Fourth Ward and West End.
larceny theft are the top crimes in Atlanta followed by aggravated assault


kable(count(COBRA_YTD2017, loc_type, sort=TRUE), "html", col.names=c("Crime Type", "Frequency")) %>%
kable_styling(bootstrap_options="striped", full_width=FALSE)
COBRA_YTD2017 %>%
  group_by(days, loc_type) %>%
  summarize(freq=n()) %>%
  ggplot(aes(reorder(days, -freq), freq)) +   
  geom_bar(aes(fill=loc_type), position="dodge", stat="identity", width=0.8, color="black") +
  xlab("Day of Week") +
  ylab("Frequency") +
  labs(fill="Crime Type") +
  ggtitle("Crime by Day of the Week")
kable
#The data provides crime type frequency and crime by day of the week.#Among the high crime categories, larceny tend to increase on Fridays and Saturdays. while burglary residence generally occurred more often during the weekdays than the weekends. Auto theft were least reported on Thursdays and increase for the weekends.





R Markdown
This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see http://rmarkdown.rstudio.com.

Note that the echo = FALSE parameter was added to the code chunk to prevent printing of the R code that generated the plot.
