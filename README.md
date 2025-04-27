# network-monitoring-dashboard
network-monitoring-dashboard
# Network Monitoring Dashboard Project (Using Python + Flask + Chart.js)

"""
Project Overview:
A simple and professional dashboard to monitor network devices' status by polling SNMP data or pulling from Zabbix API.

Technologies Used:
- Python 3.x
- Flask (for the web server)
- Chart.js (for interactive graphs)
- SNMP library: pysnmp (optional if pulling direct SNMP)
- Bootstrap 5 (for clean UI)

Main Features:
- Device health and status overview
- Real-time data visualization (uptime, CPU, memory usage)
- Alert notifications if devices are down
"""

# Project Structure:
# /network_dashboard/
# ├── app.py
# ├── templates/
# │   ├── index.html
# ├── static/
# │   ├── css/
# │   │   └── style.css
# │   ├── js/
# │   │   └── chart.js

# ===== app.py =====
from flask import Flask, render_template, jsonify
import random

app = Flask(__name__)

# Dummy Data (Simulating SNMP or API Pull)
network_devices = [
    {"name": "Router1", "status": "UP", "cpu": 23, "memory": 58},
    {"name": "Switch1", "status": "UP", "cpu": 45, "memory": 60},
    {"name": "Firewall", "status": "DOWN", "cpu": 0, "memory": 0},
]

@app.route("/")
def index():
    return render_template("index.html", devices=network_devices)

@app.route("/api/data")
def data():
    # Ideally here you'd poll SNMP or Zabbix API
    return jsonify(network_devices)

if __name__ == "__main__":
    app.run(debug=True)

# ===== templates/index.html =====
"""
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Network Monitoring Dashboard</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body>
<div class="container py-4">
  <h1 class="mb-4">Network Monitoring Dashboard</h1>
  <div class="row" id="device-status">
    <!-- Dynamic content will be inserted here -->
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
fetch('/api/data')
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('device-status');
    data.forEach(device => {
      const statusColor = device.status === "UP" ? "success" : "danger";
      container.innerHTML += `
        <div class="col-md-4">
          <div class="card text-white bg-${statusColor} mb-3">
            <div class="card-header">${device.name}</div>
            <div class="card-body">
              <h5 class="card-title">Status: ${device.status}</h5>
              <p>CPU Usage: ${device.cpu}%</p>
              <p>Memory Usage: ${device.memory}%</p>
            </div>
          </div>
        </div>
      `;
    });
  });
</script>
</body>
</html>
"""

# Final Note:
# - This is a starting point; you can extend it by pulling real SNMP data using pysnmp.
# - Integrate automatic alerts using Flask-Mail or push notifications.
# - Add authentication for dashboard access for production environments.
