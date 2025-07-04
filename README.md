# Roboshop Microservices Deployment with Ansible Roles

## Overview
Roboshop is a cloud-native, microservices-based e-commerce application. This project demonstrates how to automate the deployment and configuration of all Roboshop components using Ansible roles, following best practices for modularity, reusability, and maintainability.

---

## Project Highlights
- **Microservices Architecture:** Each business function (cart, user, catalogue, shipping, payment, etc.) is a separate service, deployed and managed independently.
- **Ansible Roles:** All automation is organized using Ansible roles, making the code modular, DRY (Don't Repeat Yourself), and easy to extend.
- **Database Diversity:** Uses MongoDB (NoSQL), MySQL (relational), and Redis (in-memory) to match each service's needs.
- **CI/CD Ready:** The structure supports automated, repeatable deployments—ideal for DevOps and cloud-native environments.
- **Best Practices:** Variables, templates, handlers, and common roles are used for safe, efficient, and scalable automation.

---

## Components & Technologies
- **Ansible:** Automation engine for configuration management and deployment.
- **Roles:** Each service (cart, user, catalogue, shipping, payment, frontend, redis, mongodb, mysql) has its own role under `roles/`.
- **Common Role:** Shared tasks (app setup, systemd, maven build, etc.) are placed in `roles/common/` and included as needed.
- **Templates:** Jinja2 templates for systemd service files and configs, with variables injected at deploy time.
- **Handlers:** Used to safely restart services only when configuration changes.
- **Databases:**
  - **MongoDB:** Used by catalogue and user services for flexible, document-based storage.
  - **MySQL:** Used by shipping and payment services for structured, transactional data.
  - **Redis:** Used by cart service for fast, temporary session storage.
- **RabbitMQ:** (Optional) For asynchronous messaging between services.
- **Maven:** Used to build Java-based services (e.g., shipping).

---

## Directory Structure (Key Parts)
```
.
├── inventory.ini                # Inventory of all hosts
├── main.yaml                    # Generic playbook to deploy any component
├── roles/
│   ├── cart/
│   ├── user/
│   ├── catalogue/
│   ├── shipping/
│   ├── payment/
│   ├── frontend/
│   ├── redis/
│   ├── mongodb/
│   ├── mysql/
│   └── common/                  # Shared tasks (appsetup, maven, systemd, etc.)
└── ...
```

---

## How It Works
1. **Inventory:** Define all your hosts and groups in `inventory.ini`.
2. **Roles:** Each service has its own role with tasks, templates, vars, and handlers.
3. **Common Role:** Shared logic (like app setup, maven build, systemd) is reused via `include_role`.
4. **Variables:** Each role's `vars/main.yaml` defines hostnames, credentials, and config values.
5. **Templates:** Jinja2 templates use variables to generate correct configs for each service.
6. **Handlers:** Services are restarted only when configs change, preventing unnecessary downtime.
7. **Playbook:** The generic `main.yaml` playbook can deploy any component by passing the `component` variable.

---

## Example: Deploying a Component
- To deploy the catalogue service:
  ```sh
  ansible-playbook -i inventory.ini -e "component=catalogue" main.yaml
  ```
- To deploy the shipping service:
  ```sh
  ansible-playbook -i inventory.ini -e "component=shipping" main.yaml
  ```
- Replace `catalogue` or `shipping` with any other component name as needed.

---

## Step-by-Step: How to Run the Project
1. **Clone the Repository:**
   ```sh
   git clone <your-repo-url>
   cd <repo-directory>
   ```
2. **Install Ansible:**
   ```sh
   # On RHEL/CentOS
   sudo dnf install ansible -y
   # Or on Ubuntu
   sudo apt-get install ansible -y
   ```
3. **Install Required Collections:**
   ```sh
   ansible-galaxy collection install community.mysql
   ```
4. **Configure Inventory:**
   - Edit `inventory.ini` to add your target hosts.
5. **Set Variables:**
   - Edit each role's `vars/main.yaml` to set hostnames, credentials, etc.
6. **Run the Playbook:**
   ```sh
   ansible-playbook -i inventory.ini -e "component=<component-name>" main.yaml
   # Example:
   ansible-playbook -i inventory.ini -e "component=frontend" main.yaml
   ```

---

## Visual Flow (How Everything Connects)
```
[inventory.ini] → [main.yaml] → [roles/<component>/tasks/main.yaml]
    ↓                ↓                ↓
[roles/common/tasks/*]   [roles/<component>/templates/*]   [roles/<component>/vars/main.yaml]
    ↓                ↓                ↓
[handlers] (restart services if needed)
```

---

## Why This Approach?
- **Modular:** Each service is independent and easy to update or scale.
- **Reusable:** Common logic is shared, reducing duplication.
- **Maintainable:** Clear structure makes troubleshooting and extending easy.
- **Production-Ready:** Follows DevOps and microservices best practices.

---

## Interview-Ready Talking Points
- Designed a full microservices deployment using Ansible roles for real-world e-commerce.
- Used best practices: modular roles, variable-driven configs, safe restarts with handlers, and DRY code with common roles.
- Automated Java builds (Maven), Python, Node.js, and database setup (MongoDB, MySQL, Redis) in a single, unified workflow.
- Easily extensible for CI/CD pipelines and cloud deployments.

---

## Credits
- Inspired by Roboshop microservices architecture.
- Automation and documentation by [Your Name].

---

## License
[MIT License](LICENSE)
