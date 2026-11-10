
## 1. Kubernetes Core Components

|Short Form / Term|Full Form|සිංහල තේරුම|
|---|---|---|
|K8s|Kubernetes|Container orchestration platform එක|
|API|Application Programming Interface|පද්ධති දෙකක් අතර සන්නිවේදනය කරන අතුරුමුහුණත|
|API Server|Kubernetes API Server|Cluster එකට ලැබෙන requests පාලනය කරන ප්‍රධාන දොරටුව|
|ETCD|Distributed Key-Value Store|Cluster එකේ තොරතුරු සහ state එක ගබඩා කරන database එක|
|Kubelet|Kubernetes Agent|Node එකේ Pods නිවැරදිව ක්‍රියාත්මක වෙනවාද බලන agent එක|
|Kube-proxy|Kubernetes Proxy|Service network traffic නිවැරදි Pods වෙත යොමු කරන component එක|
|Scheduler|kube-scheduler|Pod එකක් ක්‍රියාත්මක විය යුතු Node එක තෝරන component එක|
|CCM|Cloud Controller Manager|Cloud provider එකට අදාළ infrastructure පාලනය කරන component එක|
|CRI|Container Runtime Interface|Kubernetes සහ container runtime අතර සම්බන්ධතාව|
|CNI|Container Network Interface|Pods අතර network connectivity සකස් කරන interface එක|
|CSI|Container Storage Interface|Kubernetes වෙත storage සම්බන්ධ කරන interface එක|
|CRD|Custom Resource Definition|Kubernetes වෙත අලුත් resource වර්ග හඳුන්වා දීම|

## 2. Workloads & Controllers

|Short Form|Full Form|සිංහල තේරුම|
|---|---|---|
|Pod|Pod|Container එකක් හෝ කිහිපයක් ක්‍රියාත්මක වන කුඩාම Kubernetes unit එක|
|RS|ReplicaSet|අවශ්‍ය Pod ගණන පවත්වාගෙන යන resource එක|
|Deploy|Deployment|Applications deploy, update සහ rollback කිරීම|
|STS|StatefulSet|ස්ථිර identity සහ storage අවශ්‍ය applications පාලනය|
|DS|DaemonSet|සෑම සුදුසු Node එකකම Pod එකක් පවත්වාගෙන යාම|
|Job|Job|එක් වරක් හෝ අවශ්‍ය වාර ගණනක් සම්පූර්ණ කළ යුතු task එකක්|
|CJ|CronJob|නියමිත වේලාවකට අනුව Jobs ක්‍රියාත්මක කිරීම|
|RC|ReplicationController|පැරණි ක්‍රමයක් මගින් Pod replicas පවත්වාගෙන යාම|
|HPA|Horizontal Pod Autoscaler|CPU/Memory වැනි metrics අනුව Pod ගණන වැඩි කිරීම හෝ අඩු කිරීම|
|VPA|Vertical Pod Autoscaler|Pod එකකට ලබාදෙන CPU/Memory resources වෙනස් කිරීම|
|PDB|Pod Disruption Budget|Planned disruptions වලදී අවම Pod availability එක ආරක්ෂා කිරීම|

## 3. Kubernetes Networking

|Short Form / Term|Full Form|සිංහල තේරුම|
|---|---|---|
|SVC|Service|Pods වෙත ස්ථාවර network access එකක් ලබාදීම|
|ClusterIP|Cluster IP|Cluster එක ඇතුළත පමණක් භාවිත කළ හැකි Service IP එක|
|NodePort|Node Port|Node IP සහ port එකක් හරහා Service එකට පිවිසීම|
|LB|Load Balancer|Traffic එක backend servers/Pods අතර බෙදාහැරීම|
|Ingress|Ingress|HTTP/HTTPS traffic cluster එකට ඇතුළු කිරීම සඳහා routing rules|
|IC|Ingress Controller|Ingress rules ක්‍රියාත්මක කරන controller එක|
|DNS|Domain Name System|Domain name එක IP address එකකට පරිවර්තනය කිරීම|
|FQDN|Fully Qualified Domain Name|සම්පූර්ණ domain name එක|
|IP|Internet Protocol|Network එකක devices හඳුනාගැනීමට භාවිත කරන address එක|
|TCP|Transmission Control Protocol|විශ්වාසදායක data transmission protocol එක|
|UDP|User Datagram Protocol|Connection-oriented නොවන data transmission protocol එක|
|NAT|Network Address Translation|Network IP addresses පරිවර්තනය කිරීම|
|SNAT|Source Network Address Translation|Source IP address එක වෙනස් කිරීම|
|DNAT|Destination Network Address Translation|Destination IP address එක වෙනස් කිරීම|
|VIP|Virtual IP|Virtual ලෙස භාවිත කරන IP address එක|

## 4. Storage

|Short Form|Full Form|සිංහල තේරුම|
|---|---|---|
|PV|Persistent Volume|Kubernetes cluster එකේ ස්ථිර storage resource එක|
|PVC|Persistent Volume Claim|Pod එකකට අවශ්‍ය storage ඉල්ලීම|
|SC|StorageClass|Storage නිර්මාණය කරන ආකාරය සහ configuration එක නිර්වචනය කිරීම|
|NFS|Network File System|Network එක හරහා files share කිරීමේ ක්‍රමය|
|RWO|ReadWriteOnce|එක් Node එකකින් read/write කළ හැකි storage access mode එක|
|ROX|ReadOnlyMany|Nodes කිහිපයකින් read-only access ලබාගැනීම|
|RWX|ReadWriteMany|Nodes කිහිපයකින් read/write access ලබාගැනීම|
|RWOP|ReadWriteOncePod|එක් Pod එකකට පමණක් read/write access ලබාදීම|

## 5. Security & Access Control

|Short Form|Full Form|සිංහල තේරුම|
|---|---|---|
|CM|ConfigMap|Application configuration තබාගැනීම|
|Secret|Secret|Passwords, tokens වැනි සංවේදී දත්ත තබාගැනීම|
|SA|ServiceAccount|Pods සහ applications සඳහා identity එකක් ලබාදීම|
|RBAC|Role-Based Access Control|User කෙනෙකුට හෝ ServiceAccount එකකට ලබාදෙන permissions පාලනය|
|Role|Role|එක් Namespace එකක් තුළ permissions ලබාදීම|
|ClusterRole|Cluster Role|Cluster මට්ටමින් හෝ namespaces කිහිපයක resources සඳහා permissions ලබාදීම|
|CRB|ClusterRoleBinding|ClusterRole එකක් user/group/ServiceAccount එකකට සම්බන්ධ කිරීම|
|NP|NetworkPolicy|Pods අතර network traffic පාලනය කිරීම|
|TLS|Transport Layer Security|Network communication encrypt කර ආරක්ෂා කිරීම|
|SSL|Secure Sockets Layer|TLS වලට පෙර භාවිත වූ encryption protocol එක|
|JWT|JSON Web Token|Authentication සහ authorization සඳහා භාවිත කරන token එක|
|PSA|Pod Security Admission|Pods වල security requirements පරීක්ෂා කිරීම|

## 6. Monitoring & Troubleshooting

|Short Form / Term|Full Form|සිංහල තේරුම|
|---|---|---|
|CPU|Central Processing Unit|පරිගණකයේ processing බලය|
|RAM|Random Access Memory|තාවකාලික memory එක|
|OOM|Out Of Memory|Memory ප්‍රමාණවත් නොවීම|
|OOMKilled|Out Of Memory Killed|Memory limit ඉක්මවීම නිසා container එක terminate වීම|
|CPU Throttling|—|CPU limit එක නිසා container එකේ CPU usage සීමා කිරීම|
|Liveness Probe|—|Container එක තවමත් නිවැරදිව ක්‍රියාත්මක වෙනවාද පරීක්ෂා කිරීම|
|Readiness Probe|—|Application එක traffic ලබාගැනීමට සූදානම්ද පරීක්ෂා කිරීම|
|Startup Probe|—|Application එක ආරම්භ වී අවසන්ද පරීක්ෂා කිරීම|
|QoS|Quality of Service|Pods සඳහා ලබාදෙන resource quality classification එක|
|P95|95th Percentile|Requests වලින් 95% ක් සම්පූර්ණ වන response time සීමාව|
|P99|99th Percentile|Requests වලින් 99% ක් සම්පූර්ණ වන response time සීමාව|
|MTTR|Mean Time To Recovery|ගැටලුවකින් පසු යථා තත්ත්වයට පැමිණීමට ගතවන සාමාන්‍ය කාලය|
|SLA|Service Level Agreement|සේවා සැපයුම්කරු සහ පාරිභෝගිකයා අතර එකඟ වූ සේවා මට්ටම|
|SLO|Service Level Objective|ලබාගත යුතු සේවා මට්ටම සඳහා අභ්‍යන්තර ඉලක්කය|
|SLI|Service Level Indicator|සේවා මට්ටම මනින metric එක|

## 7. Common Pod Status & Errors

|Status / Error|සිංහල තේරුම|
|---|---|
|Pending|Pod එක තවමත් Node එකකට assign වී නැති හෝ ආරම්භ වීමට බලා සිටින තත්ත්වය|
|Running|Pod එක Node එකක ක්‍රියාත්මක වන තත්ත්වය|
|Succeeded|Pod එකේ වැඩ සාර්ථකව අවසන් වී ඇත|
|Failed|Pod එකේ ක්‍රියාවලිය අසාර්ථක වී ඇත|
|Unknown|Pod එකේ තත්ත්වය තීරණය කළ නොහැක|
|CrashLoopBackOff|Container එක නැවත නැවත crash වී restart වන තත්ත්වය|
|ImagePullBackOff|Container image එක download කිරීමට නොහැකිව නැවත උත්සාහ කරන තත්ත්වය|
|ErrImagePull|Container image එක pull කිරීමට අසමත් වීම|
|ContainerCreating|Container එක නිර්මාණය වෙමින් පවතින තත්ත්වය|
|OOMKilled|Memory සීමාව ඉක්මවීම නිසා container එක kill වීම|
|CreateContainerConfigError|Container configuration එකේ දෝෂයක් තිබීම|
|Completed|Container එකේ task එක සම්පූර්ණ වීම|
|Terminating|Pod එක delete වෙමින් පවතින තත්ත්වය|
|Evicted|Node එකේ resource ප්‍රමාණවත් නොවීම නිසා Pod එක ඉවත් කිරීම|
|Init:0/1|Init container එක තවමත් සම්පූර්ණ වී නැති බව|

වැදගත්

`Pending`, `Running`, `Succeeded`, `Failed`, `Unknown` යනු නිල Pod Phases වේ. `CrashLoopBackOff` සහ `ImagePullBackOff` යනු container waiting/error states වේ.

## 8. DevOps & Infrastructure

|Short Form|Full Form|සිංහල තේරුම|
|---|---|---|
|CI|Continuous Integration|Code changes නිතර integrate කර test කිරීම|
|CD|Continuous Delivery / Deployment|Application release සහ deployment ක්‍රියාවලිය ස්වයංක්‍රීය කිරීම|
|IaC|Infrastructure as Code|Code භාවිතයෙන් infrastructure නිර්මාණය සහ පාලනය|
|GitOps|Git-based Operations|Git repository එක භාවිතයෙන් infrastructure/application state පාලනය|
|YAML|YAML Ain't Markup Language|Configuration files ලිවීමට භාවිත කරන format එක|
|Helm|Kubernetes Package Manager|Kubernetes applications packages ලෙස deploy කිරීම|
|Kustomize|Kubernetes Configuration Customization Tool|YAML files වෙනස් කර environment අනුව භාවිත කිරීම|
|HA|High Availability|Service එකක් හැකි තරම් අඛණ්ඩව ලබාදීම|
|DR|Disaster Recovery|විපතකින් හෝ විශාල failure එකකින් පසු system එක නැවත සකස් කිරීම|
|RTO|Recovery Time Objective|System එක යථා තත්ත්වයට පත් කළ යුතු උපරිම කාලය|
|RPO|Recovery Point Objective|පිළිගත හැකි උපරිම data loss කාල පරාසය|

## 9. Essential kubectl Commands

|Command|සිංහලෙන් කරන කාර්යය|
|---|---|
|`kubectl get pods`|Pods ලැයිස්තුව පෙන්වයි|
|`kubectl get nodes`|Cluster එකේ Nodes පෙන්වයි|
|`kubectl get svc`|Services පෙන්වයි|
|`kubectl get deployments`|Deployments පෙන්වයි|
|`kubectl get all`|Namespace එකේ ප්‍රධාන resources පෙන්වයි|
|`kubectl describe pod POD_NAME`|Pod එකේ විස්තර සහ events පෙන්වයි|
|`kubectl logs POD_NAME`|Container logs පෙන්වයි|
|`kubectl logs POD_NAME --previous`|කලින් crash වූ container එකේ logs පෙන්වයි|
|`kubectl exec -it POD_NAME -- bash`|Pod එකේ container එකට ඇතුළු වීම|
|`kubectl apply -f file.yaml`|YAML file එකෙන් resource නිර්මාණය කිරීම හෝ update කිරීම|
|`kubectl delete -f file.yaml`|YAML file එකේ සඳහන් resources delete කිරීම|
|`kubectl scale deployment NAME --replicas=3`|Deployment එකේ Pod ගණන 3 දක්වා වෙනස් කිරීම|
|`kubectl rollout status deployment/NAME`|Deployment update එකේ තත්ත්වය බැලීම|
|`kubectl rollout undo deployment/NAME`|Deployment එකේ පෙර revision එකට rollback කිරීම|
|`kubectl top pods`|Pods වල CPU/Memory usage පෙන්වයි|
|`kubectl top nodes`|Nodes වල CPU/Memory usage පෙන්වයි|
|`kubectl get events`|Cluster events පෙන්වයි|
|`kubectl get namespaces`|Namespaces ලැයිස්තුගත කරයි|
|`kubectl config get-contexts`|kubectl contexts පෙන්වයි|
|`kubectl cluster-info`|Cluster එකේ ප්‍රධාන endpoint තොරතුරු පෙන්වයි|

## System Engineer කෙනෙක් අනිවාර්යයෙන් දැනගත යුතු TOP 15

මේ 15 මුලින්ම හොඳින් මතක තබාගන්න.

1. Pod – Application එක ක්‍රියාත්මක වන unit එක.
    
2. Node – Pods ක්‍රියාත්මක වන server එක.
    
3. Deployment – Application deployment සහ updates පාලනය.
    
4. Service – Pods වෙත network access ලබාදීම.
    
5. Ingress – HTTP/HTTPS routing.
    
6. ConfigMap – Configuration management.
    
7. Secret – Sensitive data management.
    
8. PV/PVC – Persistent storage.
    
9. Namespace – Cluster resources වෙන් කිරීම.
    
10. RBAC – Access permissions පාලනය.
    
11. HPA – Pod autoscaling.
    
12. Liveness Probe – Application health පරීක්ෂා කිරීම.
    
13. Readiness Probe – Traffic ලබාගැනීමට සූදානම්ද බැලීම.
    
14. CrashLoopBackOff – Container crash troubleshooting.
    
15. OOMKilled – Memory-related troubleshooting.
    

මගේ උපදෙස: මේවා නිකම්ම පාඩම් කරන්න එපා. එක් එක් concept එකට `What is it?`, `Why do we use it?`, `How does it work?`, `How do we troubleshoot it?` කියන ප්‍රශ්න හතරට උත්තර දෙන්න පුළුවන් වෙන්න ඉගෙනගන්න. එතකොට System Engineer interview එකකදීත් practical වැඩකදීත් ලේසියෙන් භාවිත කරන්න පුළුවන්.