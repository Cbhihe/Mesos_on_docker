
## A benchmark of CPU & I/O-intensive distributed computing with Docker containers on AWS

**Authors**: Name: Toni Pohl, Cédric Bhihe

### Problem Statement
Apache Spark can be run directly on/in Docker containers. However doing so is configuration intensive. For instance boot-strapping the Docker cluster requires configuration of each docker vIP address and managing the corresponding ressource allocations.  But how well can we take advantage of cluster resources across distributed applications, when relying on frameworks such as Apache Spark ?  The goal is: 
* to optimize usage (CPU and RAM)
* to ease deployment (time to deploy, configuration)
* to provide efficient ressource isolation and sharing

Using an management layer as Mesos to abstract resources eases the job and allows a more flexible management of resources for computations, e.g. with Apache Spark. 

This mini project sets out to benchmark
- CPU intensive jobs (e.g. Pi computation), as well as
- Disk-write and network-send intensive tasks (e.g. mySQL join  running a distributed computation job with Spark)

### Bibliography
- 1 paper on Apache Mesos
- 1 on Docker
- 1 on Apache Spark

### Background information on Mesos and RancerOS
#### Introduction [English]:
Apache Mesos is an open-source cluster manager that was developed at the University of California, Berkeley. It "provides efficient resource isolation, self-healing capability and sharing across distributed applications, or frameworks".[1] The software enables resource sharing in a fine-grained manner, improving cluster utilization owing to its up and down scalability
It has been adopted by several large software companies, including Twitter, Homeaway, Airbnb and Apple. At least 50 organizations currently use Mesos.[2]
On July 27, 2016 Apache Mesos announced the availability of Apache Mesos v1.0. [3] The most notable addition to the software is the “unified containerizer” that allows for the ability to centrally supply Docker, rkt and appc instances.

#### Work plan

- Setup cluster of docker container (RancherOS). / plain vanilla OS – no tweaks 
- Choice of AWS as Rancher OS image is readily available
- Deploy Apache Spark on top of Mesos
- Performance measurement (solver, network-send times)
- AWS (RTT) overhead between distributed instances of Rancher OS (straightforward ping)

#### Einführung [Deutsch]:
- Apache Mesos
Apache Mesos™ ist ein Open-Source verteilter Kernel und Scheduler für Cluster-Umgebungen, der von der Kalifornien Universität in Berkeley entwickelt wurde.

Die Software abstrahiert die darunter liegende Hardware und Infrastruktur und stellt Funktionen und APIs bereit, mit denen man standardisierte, verteilte und selbst heilende Systeme bauen kann. Die Entscheidung, welche Prozesse bzw. Anwendungen auf welchen Maschinen laufen und welche Ressourcen sie dort konsumieren, fällt die Software. Bisher wurden solche Entscheidungen in der Regel durch Menschen am Anfang eines Projekts gefällt und waren eher statisch. 

Mithilfe von Mesos kann man seine zur Verfügung stehende Infrastruktur flexibler und besser auslasten.
Benötigt eine Anwendung mehr Power, kann man diese auf mehr Maschinen verteilen. Hat eine Anwendung nichts zu tun, werden überflüssige Instanzen beendet. Mesos behält dabei den Überblick, welche Ressourcen bereits stärker oder schwächer ausgelastet sind und verteilt die Workload entsprechend.

Die Anwendungsentwickler haben klar definierte, konsistente Schnittstellen auf die Infrastruktur. Diese APIs  definieren auch die Grenze zwischen DevOps und InfrastructureOps. 
* Die Softwareentwickler beschränken sich darauf, ihre anwendungsspezifische Laufzeit einzurichten und zu betreiben.
* InfrastructureOps kann  hingegen anwendungsneutrale Laufzeitkomponenten für alle Maschinen einrichten und betreiben.
Aus Sicht der InfrastructureOps sind nun alle Maschinen gleich. Damit können auch kleine Teams eine sehr  hohe Anzahl von Maschinen betreuen. Eine sehr große Anzahl: Mesos verwaltet zum Teil Tausende von Servern für Apple Siri, AirBnb, Twitter uvm. – und das mit vergleichsweise sehr kleinen Teams.

- DataCenter Operating System (DC/OS)
DC/OS ist ein von Mesosphere entwickeltes Produkt, das auf Apache Mesos basiert. Es soll vor allem dabei helfen, den Datacenter-Betrieb zu standardisieren. Es bietet Entwicklern und Administratoren die Möglichkeit, auf sehr einfache Art und Weise komplexe, verteilte Frameworks und innovative Software-Stacks in einem Cluster zu installieren. Diese Frameworks sind bereits auf Hochverfügbarkeit und Fault Tolerance optimiert und man kann sie zum Teil ohne weitere Anpassungen für Produktionsumgebungen verwenden. Microservices und Container (z.B. Docker) werden ebenfalls besonders gut unterstützt.

Im DC/OS Universe (einer Art App Store) findet man eine wachsende Liste an Frameworks – vor allem aus  dem Big Data Umfeld, die man mit wenigen Mausklicks (oder CLI Befehlen, bzw. API Calls) fertig konfiguriert im Cluster aufstellen kann.

Einige prominente Beispiele sind:
* Spark 
* Cassandra 
* Kafka 
* Hadoop 
* Zeppelin 
* Storm 
* Swarm 
* Jenkins 

Doch DC/OS unterstützt mehr als nur die reine Installation von Paketen. Folgende Features fallen ebenfalls auf:
* Service Discovery 
* Load Balancing 
* Workload Scheduling 
* 0-Downtime Deployments 
* Umgang mit defekter Hardware 
* Monitoring und Fehleridentifikation 
* einfache Skalierbarkeit 

