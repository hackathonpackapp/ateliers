# Bienvenue dans le repo des ateliers !

## URL des machines virtuelles AWS

Chaque VM a été configurée avec une `~/.kube/config` spécifique à chaque équipe. 

Pour se connecter : 
```
ssh -i "hackathon-ec2-tp.pem" ec2-user@[URL VM GROUPE] 
```
| Equipe | IP Machine Virtuelle AWS |
|------|-------|
| packapp01 | ec2-52-210-144-207.eu-west-1.compute.amazonaws.com |
| packapp02 | ec2-34-245-116-15.eu-west-1.compute.amazonaws.com |
| packapp03 | ec2-52-49-67-109.eu-west-1.compute.amazonaws.com |
| packapp04 | ec2-34-254-157-248.eu-west-1.compute.amazonaws.com |
| packapp05 | ec2-34-254-96-133.eu-west-1.compute.amazonaws.com |
| packapp06 | ec2-34-253-192-160.eu-west-1.compute.amazonaws.com |
| packapp07 | ec2-34-252-38-170.eu-west-1.compute.amazonaws.com |
| packapp08 | ec2-34-255-136-19.eu-west-1.compute.amazonaws.com |
| packapp09 | ec2-54-229-131-76.eu-west-1.compute.amazonaws.com |
| packapp10 | ec2-34-243-151-83.eu-west-1.compute.amazonaws.com |
| packapp11 | ec2-34-244-62-229.eu-west-1.compute.amazonaws.com |
| packapp12 | ec2-34-251-254-197.eu-west-1.compute.amazonaws.com |

## Liens vers les template de déploiement

| Equipe | Template de déploiement |
|------|-------|
| packapp01 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp01.yml |
| packapp02 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp02.yml |
| packapp03 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp03.yml |
| packapp04 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp04.yml |
| packapp05 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp05.yml |
| packapp06 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp06.yml |
| packapp07 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp07.yml |
| packapp08 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp08.yml |
| packapp09 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp09.yml |
| packapp10 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp10.yml |
| packapp11 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp11.yml |
| packapp12 | https://s3-eu-west-1.amazonaws.com/hackathon-2018-deployment-templates/template-team-deployment-packapp12.yml |

## Fichiers source

**monapp-deployment.yaml**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: monapp-deployment
  namespace: packappXX
spec:
  selector:
    matchLabels:
      app: monapp
  replicas: 2
  template:
    metadata:
      labels:
        app: monapp
    spec:
      containers:
      - name: monapp
        image: registry.hackathon-container.com/packappXX/monapp:1.0
        ports:
        - containerPort: 80
      imagePullSecrets:
      - name: registry
```

**monapp-service.yaml**
```
apiVersion: v1
kind: Service
metadata:
  name: monapp-svc
  namespace: packappXX
spec:
  type: ClusterIP
  ports:
  - name: http
    port: 80
    targetPort: 80
    protocol: TCP
  selector:
    app: monapp
```

**monapp-ingress.yaml**
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: monapp
  namespace: packappXX
spec:
  rules:
  - host: monappXX.hackathon-container.com
    http:
      paths:
      - backend:
          serviceName: monapp-svc
          servicePort: 80
  tls:
  - hosts:
    - monappXX.hackathon-container.com
```

**PeristentVolumeClaim**
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: packapp00-disk
  namespace: packapp05
spec:
  storageClassName: local
  accessModes:
    - ReadWriteOnce
  selector:
    matchLabels:
      team: packapp00
  resources:
    requests:
      storage: 3Gi
```
