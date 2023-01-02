# Machine Learning Clustering Analysis 

### In the study both Hierarchical and Non Hierarchical Clustering aproaches are applied in order to analyse the nutritional properties simmilarity between Mc Donald's Hamburguer's.
***

#### Loading Libraries 

```
library(tidyverse)  | Data Manipulation Package 
library(cluster)    | Cluster Algorithm 
library(dendextend) | Compare Dendrograms 
library(factoextra) | Clustering and Visualization Algorithm
library(fpc)        | Clustering and Visualization Algorithm 
library(gridExtra)  | Grid Arrange  
library(readxl)     | Read Excel Files
```

#### Loading Database  

```
mcdonalds <- read.table("dados/MCDONALDS.csv", sep = ";", dec = ",", header = T)
```
  
#### Transforming Hamburguer Names in Lines  

```
rownames(mcdonalds) <- mcdonalds[,1]  
mcdonalds <- mcdonalds[,-1]
```
   
#### Standardizing Variables
```
mcdonalds.standardized <- scale(mcdonalds)
```
___
## Hierarchical Clustering
  
#### Using Euclidean Distance to Calculate the Matrix Distances
```
distance <- dist(mcdonalds.standardized, method = "euclidean")
```
  
#### Clustering Methods -> "average", "single", "complete" and "ward.D"

```
hierarchical.cluster <- hclust(distance, method = "single" )
```
  
#### Single Method Dendrogram

```
plot(hierarchical.cluster, cex = 0.6, hang = -1)
```

![Single Method Dendrogram](https://i.imgur.com/l8UIGCa.jpg)

#### Analysing ELBOW     

```
fviz_nbclust(mcdonalds.standardized, FUN = hcut, method = "wss")
```

![Elbow Plot](https://i.imgur.com/TSZrNzO_d.jpg?maxwidth=520&shape=thumb&fidelity=high)
  
  >Analyzing the elbow a sudden decrease in the regression is perceptible in the second and third nodes, possibly being a good statistical cut with low variation inside the cluster and high distance between the groups for our analysis.  

#### Generating 4 Hamburguer Groups 

```
hamburguer_groups <- cutree(hierarchical.cluster, k = 4)
table(hamburguer_groups)
```

>Four groups were created:    
-> First Group has 21 Hamburguers Flavors  
-> Second Group has 1 Hamburguer Flavor   
-> Third Group has 1 Hambuerguer Flavor  
-> Fourth Group has 2 Hamburguers Flavors  

#### Transforming in Dataframe the Clustering Output  

```
burguer_groups <- data.frame(hamburguer_groups)
```

#### Gathering the Cluster with the Original Database

```
mcdonalds_hamburguer_groups <- cbind(mcdonalds, burguer_groups)
```

### Descriptive Analysis  
#### Average of Variables per Group  

```
groupmean <- mcdonalds_hamburguer_groups %>% 
  group_by(hamburguer_groups) %>% 
  summarise(n = n(),
            Valor.Energetico = mean(Valor.Energetico), 
            Carboidratos = mean(Carboidratos), 
            Proteinas = mean(Proteinas),
            Gorduras.Totais = mean(Gorduras.Totais), 
            Gorduras.Saturadas = mean(Gorduras.Saturadas), 
            Gorduras.Trans = mean(Gorduras.Trans),
            Colesterol = mean(Colesterol), 
            Fibra.Alimentar = mean(Fibra.Alimentar), 
            Sodio = mean(Sodio),
            Calcio = mean(Calcio), 
            Ferro = mean(Ferro) )
data.frame(groupmean)
```

![Descriptive Cluster Analysis](https://i.imgur.com/GPDbcCJ_d.jpg?maxwidth=520&shape=thumb&fidelity=high)

____
## Non Hierarchical Clustering   

### Kmeans Clustering Model

```
mcdonalds.k2 <- kmeans(mcdonalds.standardized, centers = 2)
mcdonalds.k3 <- kmeans(mcdonalds.standardized, centers = 3)
mcdonalds.k4 <- kmeans(mcdonalds.standardized, centers = 4)
mcdonalds.k5 <- kmeans(mcdonalds.standardized, centers = 5)
```

#### Ploting in a comparative visual shape

```
G1 <- fviz_cluster(mcdonalds.k2, geom = "point", data = mcdonalds.standardized) + ggtitle("k = 2")
G2 <- fviz_cluster(mcdonalds.k3, geom = "point",  data = mcdonalds.standardized) + ggtitle("k = 3")
G3 <- fviz_cluster(mcdonalds.k4, geom = "point",  data = mcdonalds.standardized) + ggtitle("k = 4")
G4 <- fviz_cluster(mcdonalds.k5, geom = "point",  data = mcdonalds.standardized) + ggtitle("k = 5")
grid.arrange(G1, G2, G3, G4, nrow = 2)
```

![Descriptive Cluster Analysis](https://i.imgur.com/FvFmXqi_d.jpg?maxwidth=520&shape=thumb&fidelity=high)
