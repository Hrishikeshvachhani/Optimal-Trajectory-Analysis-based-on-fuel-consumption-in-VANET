# Optimal-Trajectory-Analysis-based-on-fuel-consumption-in-VANET
In this project we are going to develop a system which will be able to analyse the vehicle density at certain cluster points and ultimately visualize the fuel consumption at those points. This will help to understand the fuel consumption by the vehicles at different points and that at different period of time. So using this data we can predict the vehicle density at given period of time and thus its fuel consumption at that time. These predictions can thus help us to assist the user in selecting a better path, such that the fuel consumption can be minimized. These can be achieved at individual user level to minimize fuel consumption, as well as at higher level for an area of city or even a whole city to minimize its overall fuel consumption, as the data can be quite varied. To make this work we used time series based algorithm LSTM to predict the density of the vehicles at a given time period.

The dataset can be generated using the SUMO configuration files. We have taken Gandhinagar, Gujarat, India as the location to generate the dataset. You can chose you own location if needed. 
* ### Data Generation
Using Open Street Map tool we generated the following map for simulation.
 
The “randomTrips.py" generates a set of random trips for a given network (option -n). It does so by choosing source and destination edge either uniformly at random or with a modified distribution. The resulting trips are stored in an XML file. The following command can simulate the desired number of random trips.
python randomTrips.py -n demo.net.xml -r demo.rou.xml -e 25000 -l
demo.net.xml denotes the network file generated while generating the osm of Gandhinagar. The demo.rou.xml file is generated using this randomTrips.py file. The 25000 denotes the number of vehicles to be simulated. The entire statement thus implies that, using the network file, randomTrips.py file will generate a route file consisting of all the routes for 25000 vehicles.
The sumocfg file generated using OSM has a predefined route file by default named as “osm.passenger.trips.xml”. We have to change the route file manually in the sumocfg file as follows:
 
we have given “osm.sumocfg” file as the file to be simulated. We have specified ‘--emission-output’ as the required format of output. It generates a data where the fields are different types of energy emitted by the vehicle at each timestep. On running this python file, the SUMO application automatically opens with the specified map. On starting the simulation, it starts generating the emission file for the ongoing simulation in the xml format.
•	Pre-Processing
The file name is “xml2csv.py”. The file is dependent on another file name “xsd.py”. Both the files should be present in the very same location as the xml file that is to be converted. Following the is the command that does this conversion and as a output csv file is generated. Size of the data is reduced from 2GB to 400MB. The final dataset can be found with name “emission50000t_25000v.csv”. 
Note:- this pre-processing step is to be done in SUMO folder location.
python xml2csv.py input.xml
preprocessing.ipynb uses the emission50000t_25000v.csv and again writes into same csv after removing unwanted data.
We have used MiniBatchKMeans clustering to find the fuel cluster where the fuel is consumed most. We used Elbow method to find optimal value of the clusters. All these are done in the Clustering.ipynb. After two values of clusters 25 and 30, we get 2 csv file which contains the cluster co-ordinates timestep, vehicle density, fuel consumption. The value of vehicle density and fuel consumption are on that particular timestep. 
density_analysis_25_30.ipynp analysis the 25 and 30 cluster. It creates certain files related to the location of the cluster and files related to 0 density count. At last it creates 2 csv files corresponding to perticular cluster, which will be used by LSTM model.
We are predicting the fuel consumption using LinearSVR based on the location and density
lstm_60_5 is the LSTM model file where the model is trained.
lstm.ipynb is the testing file, where model is test on different cluster. 
