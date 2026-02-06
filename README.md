
# **AWS Cybersecurity Lab – Architecture & Rationale**

## **Overview**
This repository documents the design and deployment of a **segmented AWS VPC Cybersecurity Lab** built using Terraform. The lab provides a secure, scalable environment for offensive security testing, defensive monitoring, SOC tool evaluation, and hands‑on experimentation without exposing production systems or public cloud resources to unnecessary risk.

The architecture follows AWS best practices for **network isolation**, **least-privilege**, and **zero‑trust access**, while remaining flexible enough to support attacker-tooling, vulnerable applications, and enterprise‑grade SOC platforms.

---

# **1. Architecture Summary**

## **VPC**
- **CIDR:** `10.50.0.0/16`  
- Designed as a **fully isolated environment** dedicated to cybersecurity research and training.

## **Subnet Layout**
| Subnet Type | CIDR | Purpose |
|-------------|------|---------|
| **Public Subnet** | `10.50.1.0/24` | Bastion host + attacker workstation |
| **Private Subnet A** | `10.50.10.0/24` | Vulnerable apps (DVWA, Metasploitable, etc.) |
| **Private Subnet B** | `10.50.20.0/24` | SOC tools (Wazuh, Nessus, TheHive, Cortex, Caldera) |

## **Routing**
- Public subnet routes to **Internet Gateway**
- Private subnets route through **NAT Gateway**
- No inbound public access to any private resources

## **Security Layers**
- **Security Groups:** Strict, workload‑specific rules  
- **NACLs:** Stateless subnet‑level filtering  
- **IAM:** Least‑privilege roles for Terraform, EC2, S3, and logging  
- **VPC Endpoints:** S3, DynamoDB, CloudWatch, and others to avoid public internet traversal  

## **Access Model**
- **SSH only through Bastion Host**
- **SSM Session Manager** enabled for passwordless, keyless access
- **No public IPs** on any victim or SOC instances

---

# **2. Why This Architecture Was Chosen**

This section is written specifically for **managers, senior engineers, and executives** who want to understand the *business and security rationale* behind the design.

## **A. Security Leadership Perspective (CISO / CIO / C‑Suite)**

### **1. Risk Reduction**
- The lab is **fully isolated** from production and corporate networks.
- No private resources are exposed to the internet.
- All access is controlled through a hardened bastion and AWS SSM.

### **2. Compliance Alignment**
- Mirrors segmentation patterns used in regulated environments (HIPAA, DoD, PCI).
- Supports auditability through CloudTrail, VPC Flow Logs, and IAM least privilege.

### **3. Cost Control**
- Uses a single VPC with modular Terraform components.
- NAT Gateway and EC2 instances are right‑sized for lab workloads.
- Architecture is scalable without unnecessary baseline cost.

### **4. Strategic Value**
- Enables internal teams to train, test, and validate tools without vendor dependencies.
- Supports red‑team, blue‑team, and purple‑team exercises.
- Provides a safe environment for evaluating new security technologies.

---

## **B. Engineering Leadership Perspective (Principal Engineers / Architects)**

### **1. Clean Network Segmentation**
- Public subnet for controlled ingress.
- Private subnets for sensitive workloads.
- NAT Gateway ensures outbound access without inbound exposure.

### **2. Terraform‑Driven Infrastructure**
- Fully reproducible.
- Version‑controlled.
- Easy to tear down and rebuild for iterative testing.

### **3. Modular, Scalable Design**
- Each tool (Nessus, Wazuh, DVWA, etc.) is deployed as an independent module.
- New tools can be added without redesigning the network.

### **4. Enterprise‑Grade Patterns**
- Mirrors real‑world architectures used in SOCs and security labs.
- Supports multi‑AZ expansion if needed.

---

## **C. Manager Perspective (Team Leads / Project Managers)**

### **1. Predictable, Documented Environment**
- Clear diagrams, Terraform code, and deployment steps.
- Easy onboarding for new team members.

### **2. Supports Multiple Use Cases**
- Vulnerability scanning
- Incident response practice
- Malware analysis (isolated)
- Threat emulation
- SOC tool evaluation

### **3. Reduces External Dependencies**
- No need for third‑party lab platforms.
- Internal teams can experiment safely and quickly.

---

# **3. Tools Deployed in the Lab**

## **Attacker / Red Team**
- Ubuntu attacker workstation with Dockerized Kali tools  
- Metasploit  
- Nmap  
- Custom exploit tooling  

## **Victim / Vulnerable Apps**
- DVWA  
- Metasploitable  
- Custom vulnerable services  

## **SOC / Blue Team**
- Wazuh Manager + Agents  
- Nessus Essentials / Pro  
- TheHive  
- Cortex  
- Caldera  

---

# **4. Deployment Workflow (Terraform)**

## **1. Infrastructure Creation**
- VPC, subnets, route tables
- IGW, NAT Gateway
- Security groups
- VPC endpoints
- Bastion host
- Private EC2 instances

## **2. Automated Provisioning**
- Cloud‑init scripts for initial configuration
- Dockerized attacker tools
- Automated installation of SOC platforms

## **3. Logging & Monitoring**
- VPC Flow Logs → CloudWatch
- CloudTrail enabled
- SSM Session Manager logging

---

# **5. Benefits to the Organization**

## **Security**
- Safe environment for offensive and defensive testing  
- No risk to production systems  
- Supports continuous skill development  

## **Operational**
- Fully automated deployment  
- Easy to rebuild, reset, or expand  
- Consistent environment for all team members  

## **Strategic**
- Demonstrates cloud security maturity  
- Enables internal R&D  
- Reduces reliance on external training platforms  

---

# **6. Future Enhancements**
- Multi‑AZ expansion  
- Automated blue‑team alert pipelines  
- Integration with SIEM platforms  
- Automated red‑team scenarios (Atomic Red Team, Caldera profiles)  
- Containerized SOC stack for faster rebuilds  

---

# **7. Conclusion**

This VPC Cyber Lab provides a **secure, scalable, and enterprise‑grade environment** for cybersecurity training, testing, and research. It reflects modern cloud security architecture principles and demonstrates the organization’s commitment to building internal capability while minimizing risk and cost.

---

