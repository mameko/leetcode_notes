# 134 Gas Station

There areNgas stations along a circular route, where the amount of gas at stationiis`gas[i]`.

You have a car with an unlimited gas tank and it costs`cost[i]`of gas to travel from stationito its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

**Note:**\
The solution is guaranteed to be unique.

阅读并背诵全文

```java
public int canCompleteCircuit(int[] gas, int[] cost) {
     if(gas==null||gas.length==0||cost==null||cost.length==0||gas.length!=cost.length){
        return -1;
    }

    int totalGas = 0;
    int totalCost = 0;
    int start = 0;
    int runningSum = 0;

    for(int i=0;i<gas.length;i++){
        totalGas = gas[i]+totalGas;
        totalCost = cost[i]+totalCost;
        runningSum = runningSum + gas[i] - cost[i];

        if(runningSum < 0){
            start = i + 1;
            runningSum = 0;
        }
    }


    if(totalGas < totalCost){
        return -1;
    }

    return start;
}
```
