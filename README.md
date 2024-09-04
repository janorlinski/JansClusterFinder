# JansClusterFinder
Simple T0 cluster finder. Among all hits within an event, it will find groups that lie within a specified time from each other and collect them into a cluster. Then, it will find the max-ToT hit within that group and save its information as global properties of the cluster.

## How to use the cluster finder
First, declare and define the cluster, while specifiyng the time window. Keep in mind, that this time window (a) will go both ways (i.e. giving a time window of 5 ns will accept hits that have a relative distance of <= 5 ns from each other, which means a total time window of 10 ns) and (b) be calculated for all hits within a cluster candidate (not just from the first hit). Also, keep in mind that for now the cluster finder does not differentiate between modules, so please have a separate cluster finder for each module. Of course, you need a fresh finder for each event.

```
JansClusterFinder clfMod0(5.0);
JansClusterFinder clfMod1(5.0);
JansClusterFinder clfMod3(5.0);
```

Then, when looping over events, fill the cluster finder with T0 hits. 

```	
if (moduleRef == 0) clfMod0.addHit(moduleRef, indexRef, widthRef, timeRef);
if (moduleRef == 1) clfMod1.addHit(moduleRef, indexRef, widthRef, timeRef);
if (moduleRef == 3) clfMod3.addHit(moduleRef, indexRef, widthRef, timeRef);
```

Once all hits are filled - you can find clusters within them. 

```
clfMod0.findClusters();
clfMod1.findClusters();
clfMod3.findClusters();
```

Access to the clusters is done as follows (example for module 0). First obtain the total number of found clusters with `clfMod0.getNumberOfClusters()`. Then, you can loop over the clusters and access their parameters with a simple integer index (like in a for loop):

```
for (Int_t i=0; i<clfMod0.getNumberOfClusters(); i++) {
  cout << "cluster in mod 0, tot = " << clfMod0.getClusterTot(i) << ", time = " << clfMod0.getClusterTime(i) << " and clustersize = " << clfMod0.getClusterSize(i) << endl;
}

And thats it!

##

What is missing (features that would be nice to add):
 - differentiate by module in the cluster finding algorithm - then you would only need one cluster finder per event
 - optimize the algorithm
