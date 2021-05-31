# VehicleRoutingProblem

Solution Background & Problem Statement:

As many companies focus on their core business, warehousing and transport activities are many times outsourced to third-party logistics providers. Road transport is still the most used mode of transport for distribution of goods. Logistics providers want to use their fleet of vehicles in the most efficient way, so route optimisation comes into the picture. In the operations research literature, the Vehicle Routing Problem (VRP) is one of the most studied problems because of both its practical relevance and its computational complexity. The issue concerns the distribution of goods between hubs and customers along a set of routes for a fleet of vehicles where an objective function is optimized. The objective function could me minimizing total routing cost,  minimizing total distance travelled, etc, while taking into real life constraints like maximising capacity utilization of all the fleet, meeting customer demand, ensuring timely delivery, etc. Solving a basic vehicle routing problem involves two elements: the assignment of all customers to a trip and the sequence in which each customer or location is visited.  
Real-life cases face uncertainties in load of orders, in travel time and in availability of the vehicles. The route planner may be confronted with data disturbances in the routine activities. An optimal solution produced by using deterministic input variables may be infeasible or not good enough in case a situation has changed.

Thus, we would like to focus on a variant of the Vehicle Routing Problem – i.e. Clustered Vehicle Routing Problem, where customers are divided into clusters based on geo location. While doing so, the solution may determine vehicles to consecutively visit all customers belonging to the same cluster. This approach may work very well for clusters where the load of delivering orders is high, however, fewer orders scattered across multiple clusters may not able to meet the constraint of ensuring the fleet capacity if optimally utilized. For solving this problem, there is a need to: 
1.	Automatically create clusters based on load (no. of orders to be picked up / delivered) in a given hub where route planning is being done
2.	Sort all orders based on the clusters being defines
3.	Create trips with definitive routes for the vehicles and assign all orders to these trips

Expectations: 
1. Select the most suited solution(s) for route planning & clustering from GitHub for the above stated problem
2. Build a solution for automating generation of clusters based on load (no. of orders to be picked up / delivered)
3. Run a simulation for 5000 orders to be delivered to different locations in a single day in a particular geographical territory with multiple clusters using a fixed number of vehicles. By minimizing the total distance travelled by all vehicles & ensuring maximum capacity of each of the vehicles assigned to trips is utilized. 
4. Assume 100 vehicles available for the route planning at the hub
5. Output of the simulation – 
- Each of the 5000 order must be assigned to a trip (which is assigned to a single Vehicle)
- Each trip must have routes & sequencing of orders defined to minimize the amount of distance travelled. 
6. The project must be accompanied with a summary document of the open source projects used and what changes were implemented or what modules were built to arrive at the simulation results


------------------------------------------------------------------------------------

## Example
Input data: [`./data/location.csv`](./data/locations.csv)
* Format: latitude,longitude,is_customer(0 or 1)
* Number of Locations(*__N__*) = 537
* Number of Vehicles(*__V__*) = 25
* For each location
  * Demand(*__d<sub>i</sub>__*) = 1
* The capacity of the vehicle(*__c__*) = 21 ~ 25 (= 537 / 25)

## Proposed Solution
The vehicle routing problem (VRP) is a NP(Nondeterministic Polymomial-time) hard problem.
This project uses a heurisitic algorithm to solve this problem. The proposed algorithm consists of two phases:
* Phase1. Finds the centroids which will be delivered by each vehicle by using k-means clustering algorithm
* Phase2. Assigns the customers to each centroid(i.e. vehecle)
* Phase3. Optimize the vehicle routing(This is a TSP problem.)

Also, Phase1,2 algorithm can be applicable to the multiple warehouses problem with same algorithm. i.e., the vehicles can be replaced with the warehouses.


### The input data
<table style="vertical-align:middle; text-align:center; border-collapse:collapse;">
<tr>
<td style="border:none"><img src="./images/figure_01_input.png" style="height:auto; max-width:80%;"></td>
</tr>
</table>
 
### The clustered centroids(= vehecles)
Initialize and sort the cluster centroids by the farthest order of the distance between the warehouse and centroid. i.e. The sorted cluster centroids are the vehicles to assign the customers.
We assume that each vehicle's max capacity is 21 ~ 25. (i.e. capacity = number of customers / number of vehicles)
<table style="vertical-align:middle; text-align:center; border-collapse:collapse;">
<tr>
<td style="border:none"><img src="./images/figure_02_clustered_centroids.png" style="height:auto; max-width:100%;"></td>
</tr>
</table>

### The assigned customers
Assigns all the customers into each cluster centroid(i.e. vehicle) by the farthest order of the centroids subject to the constraint of vehicle's capacity.
<table style="vertical-align:middle; text-align:center; border-collapse:collapse;">
<tr>
<td style="border:none"><img src="./images/figure_03_assigned_customers.png" style="height:auto; max-width:100%;"></td>
</tr>
</table>

### The optimized vehicles routing
First opimizes vehicles routing by greedy solution (nearest neighbor).
It starts from 0, add nearest neighbor to the cycle at each step.
Finally opimizes the routing by using 2-opt solution.
<table style="vertical-align:middle; text-align:center; border-collapse:collapse;">
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_01.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_02.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_03.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_04.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_05.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_06.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_07.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_08.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_09.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_10.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_11.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_12.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_13.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_14.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_15.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_16.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_17.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_18.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_19.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_20.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_21.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_22.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_23.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_24.png" style="height:auto; max-width:100%;"></td>
</tr>
<tr>
<td style="border:none"><img src="./images/figure_04_vehicle_25.png" style="height:auto; max-width:100%;"></td>
</tr>
</table>
</nobr></center></div>

### Output
```
total cost: 3.43503
vehicle1: 0 208 274 53 9 10 6 7 4 2 1 3 15 16 22 23 25 26 28 32 30 31 0
vehicle2: 0 443 422 426 431 441 478 498 499 509 522 516 534 535 536 525 526 524 528 529 530 527 0
vehicle3: 0 51 29 21 19 14 12 11 5 172 160 195 264 348 344 349 333 343 200 199 132 222 0
vehicle4: 0 289 288 249 223 201 131 156 161 150 149 141 140 84 88 71 55 57 48 18 20 13 0
vehicle5: 0 503 501 494 489 490 491 488 497 496 502 505 517 520 519 510 518 511 512 523 531 532 0
vehicle6: 0 457 463 476 483 479 459 444 442 453 456 471 474 447 355 358 379 402 401 390 400 404 0
vehicle7: 0 353 384 286 228 213 206 209 196 187 184 180 178 170 165 155 154 153 152 112 101 73 0
vehicle8: 0 375 408 437 439 438 448 475 533 521 515 514 513 507 508 506 504 485 473 458 451 424 0
vehicle9: 0 397 405 413 434 449 469 433 435 436 386 376 360 336 271 181 157 142 133 129 119 74 0
vehicle10: 0 462 466 467 468 481 482 484 486 493 500 495 492 487 480 477 472 470 414 415 407 454 0
vehicle11: 0 347 380 406 420 419 417 388 389 337 387 385 374 361 352 351 295 291 255 215 235 270 0
vehicle12: 0 128 127 104 95 94 89 87 79 75 69 60 63 59 49 42 27 24 34 39 52 70 0
vehicle13: 0 329 290 281 285 238 237 204 216 217 192 159 158 137 136 144 120 123 122 107 108 96 0
vehicle14: 0 166 124 130 125 151 44 38 41 35 40 37 36 17 8 43 54 33 248 268 231 210 0
vehicle15: 0 429 427 428 425 430 446 450 452 460 461 465 464 455 445 432 423 418 410 409 399 363 0
vehicle16: 0 365 369 373 383 378 371 377 391 392 393 395 394 396 398 412 421 440 416 411 403 382 0
vehicle17: 0 307 319 318 316 315 323 339 356 359 364 368 367 381 372 299 296 294 292 279 278 265 0
vehicle18: 0 328 335 334 325 324 313 306 304 305 321 320 322 338 340 341 354 366 370 362 342 357 0
vehicle19: 0 254 266 218 226 225 224 190 174 167 179 169 110 109 105 102 98 97 111 113 147 148 0
vehicle20: 0 103 92 91 80 82 77 72 65 64 58 46 45 56 62 78 76 83 85 100 106 114 327 0
vehicle21: 0 308 309 331 332 346 350 345 330 326 317 310 311 300 283 287 282 276 263 267 273 303 302 293 0
vehicle22: 0 314 312 146 138 139 126 121 117 116 118 86 66 61 50 47 68 67 81 90 93 99 115 135 0
vehicle23: 0 242 247 261 262 229 214 211 212 205 202 219 221 227 239 241 232 243 258 259 260 272 297 284 0
vehicle24: 0 176 175 163 164 168 162 145 134 143 177 173 183 197 198 194 193 185 186 188 207 240 298 301 0
vehicle25: 0 203 182 171 189 191 220 230 233 234 236 244 245 246 253 252 251 250 277 280 275 269 257 256 0
```

## References

* [A Centroid-based heuristic algorithm for the capacitated vehicle routing problem](https://www.google.co.id/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjbiaahgvTWAhVFQo8KHT20DYYQFggqMAA&url=http%3A%2F%2Fwww.cai.sk%2Fojs%2Findex.php%2Fcai%2Farticle%2Fdownload%2F192%2F162&usg=AOvVaw1qRMXhuzaCZcEOP5oT9XtW), by K Shin, 2012

* [Solving Multiple TSP Problem by K-Means and Crossover based Modified ACO Algorithm](https://www.researchgate.net/profile/Majd_Latah/publication/316855410_Solving_multiple_tsp_problem_by_k-means_and_crossover_based_modified_aco_algorithm/links/591438a6aca27200fe4ccc4b/Solving-multiple-tsp-problem-by-k-means-and-crossover-based-modified-aco-algorithm.pdf), by Majd Latah, 2016

* [2-opt algorithm](https://en.wikipedia.org/wiki/2-opt), in Wikipedia

