# Hybrid Enterprise Lab — On-prem + Cloud Infrastructure

Projekt  hybrydowej infrastruktury, która łączy środowisko lokalne (on-prem, Proxmox) z chmurą (AWS lub Azure) przy użyciu VPN i automatyzacji.

---

## Architektura

**On-prem (mini PC / Proxmox):**
- pfSense – firewall, VPN, DNS, site-to-site tunnel do chmury  
- Jump Host – Ansible, Terraform, Git, sterowanie infrastrukturą  
- Repozytorium (Apt-Cacher) – lokalne aktualizacje offline    
- Zabbix Proxy – bufor monitoringu  
- Agenty: Wazuh, Fluent Bit, Node Exporter  

**Cloud (uruchamiane tylko gdy potrzeba):**
- Wazuh + OpenSearch – SIEM (lekka wersja bez pełnego ELK)  
- Grafana + Loki + Prometheus – monitoring i wizualizacja metryk  
- Zabbix Server – centralny monitoring  

---

## Cele projektu

- Projekt infrastruktury enterprise  
- Automatyzacja z Ansible (Infrastructure as Code)  
- Praca w trybie offline (air-gap) z lokalnym repozytorium  
- Hybrydowy monitoring i bezpieczeństwo  
- Dokumentacja i GitOps workflow  

---

## Struktura projektu

hybrid-enterprise-lab/
├── ansible/ → playbooki i role do automatyzacji
├── docker/ → konfiguracje docker-compose (Grafana, Wazuh, Prometheus)
├── docs/ → dokumentacja krok po kroku
├── inventory/ → adresy serwerów i pliki inwentarza
└── README.md → opis projektu