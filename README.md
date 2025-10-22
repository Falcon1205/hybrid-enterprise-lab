# Hybrid Enterprise Lab — On-prem + Cloud Infrastructure

Projekt edukacyjny odzwierciedlający środowisko korporacyjne (enterprise).  
**hybrydowa infrastruktura**, która łączy środowisko lokalne (Proxmox) z chmurą (AWS / Azure) przy użyciu VPN i automatyzacji.

---

## Architektura

**On-prem (mini PC / Proxmox):**
- pfSense – firewall, VPN, DNS, site-to-site tunnel do chmury  
- Jump Host – Ansible, Terraform, GitOps control node  
- Repozytorium (Apt-Cacher) – lokalne aktualizacje offline  
- Gitea – lokalny Git server (mirror z GitHub)  
- Zabbix Proxy – bufor monitoringu  
- Agenty: Wazuh, Fluent Bit, Node Exporter  

**Cloud (uruchamiane tylko gdy potrzeba):**
- Wazuh + OpenSearch – SIEM (lekka wersja)  
- Grafana + Loki + Prometheus – monitoring i metryki  
- (opcjonalnie) Zabbix Server – centralny monitoring  

---

## Stos technologiczny

| Kategoria | Technologia |
|------------|--------------|
| Wirtualizacja | Proxmox VE |
| Firewall / VPN | pfSense |
| IaC / Automatyzacja | Ansible, Terraform |
| Repozytorium kodu | Gitea + GitHub |
| Monitoring | Zabbix, Grafana, Prometheus |
| Logowanie / SIEM | Wazuh, Fluent Bit, OpenSearch |
| Systemy operacyjne | Debian / Ubuntu Server |
| CI/CD | Gitea Actions / Jenkins (opcjonalnie) |

---

## Diagram architektury
<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/1b4ba619-2b8c-4bb2-8b78-fead2bb4e290" />



---

