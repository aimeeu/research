Installing and configuring HashiCorp Nomad in a production environment on Linux involves several steps to ensure a secure, scalable, and reliable setup. Below is a concise guide to the process, assuming a Linux distribution like Ubuntu or CentOS. This guide covers setting up a Nomad cluster with servers and clients, securing it, and integrating it with Consul for service discovery (optional but recommended for production).

Steps to Install and Configure HashiCorp Nomad in a Production Environment
1. Plan the Cluster Architecture
	•	Determine Node Roles:
	◦	Nomad Servers: Manage the cluster state and scheduling (typically 3 or 5 for high availability).
	◦	Nomad Clients: Run workloads (can scale as needed).
	•	Hardware Requirements:
	◦	Servers: Minimum 2 CPU cores, 4 GB RAM, SSD storage.
	◦	Clients: Varies based on workload; ensure sufficient CPU, memory, and disk.
	•	Networking:
	◦	Ensure low-latency communication between nodes.
	◦	Open ports: 4646 (HTTP API), 4647 (RPC), 4648 (Serf LAN for gossip).
	•	Optional Integration:
	◦	Plan to integrate with Consul for service discovery and health checking.
	◦	Consider Vault for secret management.
2. Install Nomad
	•	On Each Node (Servers and Clients):
	1	Download Nomad:
	▪	Visit the HashiCorp Nomad downloads page.
	▪	For the latest version (e.g., 1.9.2 as of July 2025), use: wget https://releases.hashicorp.com/nomad/1.9.2/nomad_1.9.2_linux_amd64.zip
	▪	
	2	Extract and Install: unzip nomad_1.9.2_linux_amd64.zip
	3	sudo mv nomad /usr/local/bin/
	4	
	5	Verify Installation: nomad version
	6	 Ensure the output shows the installed version (e.g., Nomad v1.9.2).
	7	Create Nomad User (for security): sudo useradd --system --home /etc/nomad.d --shell /bin/false nomad
	8	
	9	Set Up Directories: sudo mkdir -p /etc/nomad.d /var/lib/nomad
	10	sudo chown nomad:nomad /etc/nomad.d /var/lib/nomad
	11	
3. Configure Nomad Servers
	•	Create Server Configuration:
	◦	On each server node, create /etc/nomad.d/nomad.hcl: datacenter = "dc1"
	◦	data_dir   = "/var/lib/nomad"
	◦	
	◦	server {
	◦	  enabled          = true
	◦	  bootstrap_expect = 3 # Number of expected servers
	◦	  encrypt          = "" # Generate with `nomad operator keygen`
	◦	}
	◦	
	◦	bind_addr = "0.0.0.0"
	◦	advertise {
	◦	  http = ""
	◦	  rpc  = ""
	◦	  serf = ""
	◦	}
	◦	
	◦	consul {
	◦	  address = ":8500"
	◦	  token   = "" # Optional, for secure Consul integration
	◦	}
	◦	
	◦	telemetry {
	◦	  prometheus_metrics = true
	◦	  disable_hostname   = true
	◦	}
	◦	
	◦	Replace with the node’s IP address.
	◦	Generate a gossip encryption key: nomad operator keygen.
	◦	If using Consul, specify the Consul server’s IP and a client token.
	•	Enable High Availability:
	◦	Set bootstrap_expect to the number of server nodes (e.g., 3 or 5 for fault tolerance).
	◦	Ensure servers can communicate over ports 4647 (RPC) and 4648 (Serf).
4. Configure Nomad Clients
	•	Create Client Configuration:
	◦	On each client node, create /etc/nomad.d/nomad.hcl: datacenter = "dc1"
	◦	data_dir   = "/var/lib/nomad"
	◦	
	◦	client {
	◦	  enabled = true
	◦	  servers = [":4647", ":4647", ":4647"]
	◦	}
	◦	
	◦	bind_addr = "0.0.0.0"
	◦	advertise {
	◦	  http = ""
	◦	  rpc  = ""
	◦	  serf = ""
	◦	}
	◦	
	◦	consul {
	◦	  address = ":8500"
	◦	  token   = "" # Optional
	◦	}
	◦	
	◦	telemetry {
	◦	  prometheus_metrics = true
	◦	  disable_hostname   = true
	◦	}
	◦	
	◦	Replace , , with the IPs of Nomad servers.
	◦	Replace with the client node’s IP.
	•	Install Dependencies:
	◦	For Docker workloads, install Docker: sudo apt-get update
	◦	sudo apt-get install -y docker.io
	◦	sudo systemctl enable docker
	◦	sudo systemctl start docker
	◦	
	◦	For other drivers (e.g., Java, Podman), install them as needed.
5. Set Up Systemd Service
	•	Create Systemd Unit File:
	◦	On all nodes, create /etc/systemd/system/nomad.service: [Unit]
	◦	Description=Nomad
	◦	Documentation=https://www.nomadproject.io/docs/
	◦	Wants=network-online.target
	◦	After=network-online.target
	◦	
	◦	[Service]
	◦	User=nomad
	◦	Group=nomad
	◦	ExecStart=/usr/local/bin/nomad agent -config=/etc/nomad.d/nomad.hcl
	◦	ExecReload=/bin/kill -HUP $MAINPID
	◦	KillMode=process
	◦	Restart=on-failure
	◦	LimitNOFILE=65536
	◦	
	◦	[Install]
	◦	WantedBy=multi-user.target
	◦	
	•	Enable and Start Nomad: sudo systemctl enable nomad
	•	sudo systemctl start nomad
	•	
	•	Verify Service: sudo systemctl status nomad
	•	
6. Secure the Cluster
	•	Enable Gossip Encryption:
	◦	Use the gossip key generated earlier in the encrypt field of the server and client configurations.
	•	Enable TLS:
	◦	Generate certificates for HTTP and RPC communication using a trusted CA or tools like cfssl.
	◦	Update nomad.hcl with: tls {
	◦	  http = true
	◦	  rpc  = true
	◦	  ca_file   = "/etc/nomad.d/nomad-ca.pem"
	◦	  cert_file = "/etc/nomad.d/server.pem"
	◦	  key_file  = "/etc/nomad.d/server-key.pem"
	◦	}
	◦	
	•	ACLs (Access Control Lists):
	◦	Enable ACLs for secure access: acl {
	◦	  enabled = true
	◦	}
	◦	
	◦	Bootstrap ACLs on one server: nomad acl bootstrap
	◦	
	◦	Save the generated token and use it to create policies and tokens for users and services.
7. Integrate with Consul (Optional but Recommended)
	•	Install Consul:
	◦	Follow similar steps to install Consul on all nodes (or dedicated Consul servers).
	◦	Configure Consul agents with a similar datacenter name as Nomad.
	•	Register Services:
	◦	Nomad automatically registers jobs with Consul if the consul block is configured.
	◦	Ensure Consul’s HTTP API (8500) is accessible.
	•	Verify Integration: consul members
	•	nomad node status
	•	
8. Test the Cluster
	•	Verify Cluster Health: nomad server members
	•	nomad node status
	•	
	•	Deploy a Test Job:
	◦	Create a sample job file (example.nomad): job "example" {
	◦	  datacenters = ["dc1"]
	◦	  type = "service"
	◦	  group "cache" {
	◦	    count = 1
	◦	    task "redis" {
	◦	      driver = "docker"
	◦	      config {
	◦	        image = "redis:7"
	◦	        ports = ["redis"]
	◦	      }
	◦	      resources {
	◦	        cpu    = 500
	◦	        memory = 256
	◦	      }
	◦	    }
	◦	    network {
	◦	      port "redis" {
	◦	        static = 6379
	◦	      }
	◦	    }
	◦	  }
	◦	}
	◦	
	◦	Run the job: nomad job run example.nomad
	◦	
	◦	Check job status: nomad job status example
	◦	
9. Monitoring and Logging
	•	Enable Telemetry:
	◦	Use the telemetry block to expose metrics for Prometheus.
	•	Set Up Logging:
	◦	Configure centralized logging (e.g., ELK stack or Loki) to collect Nomad logs from /var/log/nomad.
	•	Monitor Cluster:
	◦	Use Prometheus and Grafana for metrics visualization.
	◦	Check Nomad’s UI at http://:4646.
10. Backup and Maintenance
	•	Backup Cluster State:
	◦	Periodically snapshot the cluster: nomad operator snapshot save backup.snap
	◦	
	•	Update Nomad:
	◦	Upgrade Nomad by replacing the binary and restarting the service: sudo systemctl stop nomad
	◦	wget 
	◦	unzip nomad__linux_amd64.zip
	◦	sudo mv nomad /usr/local/bin/
	◦	sudo systemctl start nomad
	◦	
	•	Scale the Cluster:
	◦	Add more client nodes by repeating the client setup steps.
	◦	Monitor resource usage and adjust as needed.

Additional Considerations
	•	High Availability: Use an odd number of servers (3 or 5) to avoid split-brain scenarios.
	•	Storage: Ensure /var/lib/nomad is on a reliable, high-performance disk.
	•	Firewall: Restrict access to Nomad ports (4646–4648) to trusted networks.
	•	Consul Integration: Strongly recommended for service discovery and health checks in production.
	•	Vault Integration: Use Vault for secret management if sensitive data is involved.
	•	Documentation: Refer to Nomad’s official documentation for advanced configurations.
By following these steps, you’ll have a production-ready Nomad cluster with security, scalability, and monitoring in place. Let me know if you need help with specific configurations or troubleshooting!
