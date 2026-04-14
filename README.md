<h1>🚀 Project 6 — Deploy a Real Application Using Helm (Python App)</h1>

<p><strong>Let’s gooo 🔥</strong> — This project takes you into <b>real-world Kubernetes workflows</b>.</p>

<p>This is not just YAML anymore.<br>
This is how teams deploy applications in <b>production using Helm</b>.</p>

<hr>

<h2>🎯 Goal (Real DevOps Context)</h2>

<ul>
  <li>Build a <b>Python (Flask) application</b></li>
  <li>Containerize it using Docker</li>
  <li>Package Kubernetes configs using <b>Helm</b></li>
  <li>Deploy using Helm (industry standard)</li>
</ul>

<p>👉 Helm = <b>“npm/yum for Kubernetes”</b></p>

<hr>

<h2>🧠 Architecture</h2>

<p><b>Flow:</b></p>

<pre>
Helm Chart → Kubernetes → Deployment → Pod → Service → User
</pre>

<hr>

<h2>📁 Project Structure</h2>

<pre>
kubernetes-project6/
│
├── app/
│   ├── app.py
│   └── requirements.txt
│
├── Dockerfile
│
└── helm-chart/
    └── python-app/
        ├── Chart.yaml
        ├── values.yaml
        └── templates/
            ├── deployment.yaml
            ├── service.yaml
            └── _helpers.tpl
</pre>

<hr>

<h2>🧩 Step 1 — Python Application</h2>

<h3>📄 app/app.py</h3>

<pre><code>from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Kubernetes Helm 🚀"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
</code></pre>

<h3>📄 requirements.txt</h3>

<pre><code>flask
</code></pre>

<hr>

<h2>🐳 Step 2 — Dockerfile</h2>

<pre><code>FROM python:3.9-slim

WORKDIR /app

COPY app/ .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
</code></pre>

<h3>🚀 Build Image (Inside Minikube)</h3>

<pre><code>eval $(minikube docker-env)

docker build -t python-helm-app:v1 .
</code></pre>

<hr>

<h2>📦 Step 3 — Create Helm Chart</h2>

<pre><code>helm create python-app
mv python-app helm-chart/
</code></pre>

<hr>

<h2>📄 Step 4 — Chart.yaml</h2>

<pre><code>apiVersion: v2
name: python-app
description: A Helm chart for Kubernetes Python app
version: 0.1.0
appVersion: "1.0"
</code></pre>

<hr>

<h2>⚙️ Step 5 — values.yaml (CONTROL CENTER)</h2>

<pre><code>replicaCount: 2

image:
  repository: python-helm-app
  tag: v1
  pullPolicy: IfNotPresent

service:
  type: NodePort
  port: 5000
  nodePort: 30009
</code></pre>

<hr>

<h2>📄 Step 6 — Deployment Template</h2>

<pre><code>apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-app
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: python-app
  template:
    metadata:
      labels:
        app: python-app
    spec:
      containers:
      - name: python-container
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: 5000
</code></pre>

<hr>

<h2>🌐 Step 7 — Service Template</h2>

<pre><code>apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-service
spec:
  type: {{ .Values.service.type }}
  selector:
    app: python-app
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 5000
      nodePort: {{ .Values.service.nodePort }}
</code></pre>

<hr>

<h2>🚀 Step 8 — Deploy Using Helm</h2>

<pre><code>helm install my-python-app helm-chart/python-app
kubectl get all
</code></pre>

<hr>

<h2>🌐 Access Application</h2>

<pre><code>minikube service my-python-app-service
</code></pre>

<p>👉 Output:</p>

<pre><code>Hello from Kubernetes Helm 🚀
</code></pre>

<hr>

<h2>🔄 Step 9 — Upgrade (Why Helm is Powerful)</h2>

<p>Update replicas:</p>

<pre><code>replicaCount: 3
</code></pre>

<p>Apply upgrade:</p>

<pre><code>helm upgrade my-python-app helm-chart/python-app
</code></pre>

<p>👉 Zero-downtime deployment 🔥</p>

<hr>

<h2>🧠 What You Learned</h2>

<ul>
  <li>Helm chart structure</li>
  <li>Templating with <code>{{ }}</code></li>
  <li><code>values.yaml</code> as control center</li>
  <li>Upgrade vs redeploy</li>
  <li>Real-world deployment workflow</li>
</ul>

<hr>

<h2>❌ Common Mistakes</h2>

<ul>
  <li>Wrong image name</li>
  <li>Not building inside Minikube</li>
  <li>Template syntax errors</li>
  <li>Port mismatch</li>
</ul>

<p><b>Debug:</b></p>

<pre><code>helm template my-python-app helm-chart/python-app
</code></pre>

<hr>

<h2>🚀 Upgrade Ideas</h2>

<ul>
  <li>Add Ingress</li>
  <li>Add ConfigMap via Helm</li>
  <li>Add liveness/readiness probes</li>
  <li>Use Helm dependencies (Database)</li>
</ul>

<hr>

<h2>🎯 Interview Killer Answer</h2>

<p><b>Q: Why Helm?</b></p>

<blockquote>
Helm standardizes Kubernetes deployments using reusable templates and values, enabling versioning, upgrades, and consistency across environments.
</blockquote>

<hr>

<h2>🧭 Your Task</h2>

<ol>
  <li>Push full Helm structure to repo</li>
  <li>Build image in Minikube</li>
  <li>Install Helm chart</li>
  <li>Try upgrade</li>
</ol>

<hr>

<h2>🔥 Next Step</h2>

<p>When you say <b>“Project 6 done”</b>, you will:</p>

<ul>
  <li>Get a senior-level DevOps review</li>
  <li>Learn production-grade improvements</li>
  <li>Move to <b>Project 7 (Autoscaling — HPA)</b></li>
</ul>

<hr>

<p><b>You’ve officially entered real DevOps territory 🚀</b></p>
