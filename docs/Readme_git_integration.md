# GitHub ↔ Gitea Integration (Hybrid Enterprise Lab)

## Cel

Celem tej konfiguracji jest utworzenie **bezpiecznego i realistycznego workflow Git** pomiędzy:
- **publicznym repozytorium GitHub** – używanym jako portfolio i źródło wersji,
- **wewnętrznym serwerem Gitea** w strefie DMZ – używanym jako mirror i repozytorium operacyjne,
- **Jump Hostem** – który automatyzuje wdrożenia z Gitei (Ansible / Terraform).

To odzwierciedla praktyczny model _Dev → Integration → Controlled Deployment_ stosowany w środowiskach enterprise.

---

## Architektura

| Warstwa | Komponenty | Sieć |
|----------|-------------|------|
| **Admin Workstation (VPN Client)** | Git, SSH, Ansible CLI | zdalny dostęp przez VPN |
| **pfSense** | Firewall, VPN Gateway | LAN ↔ DMZ ↔ VPN |
| **LAN (10.10.10.0/24)** | Jump Host (Ansible/Terraform), User VM | zarządzanie |
| **DMZ (10.10.30.0/24)** | Gitea, Repo Server, Zabbix Proxy | usługi wewnętrzne |
| **GitHub (Internet)** | Repo publiczne: [`Falcon1205/hybrid-enterprise-lab`](https://github.com/Falcon1205/hybrid-enterprise-lab) | publiczny Dev repo |

---

## Etap 1 – Połączenie przez VPN

- **pfSense** skonfigurowano jako serwer **OpenVPN**.  
- W polu **IPv4 Local Networks** dodano:
10.10.10.0/24,10.10.30.0/24

- W **Firewall → Rules → OpenVPN** dodano regułę `allow any → any`.  
- W **Firewall → Rules → DMZ** dodano:
Allow TCP 2222 from VPN subnet to Gitea (10.10.30.11)


Efekt: po zalogowaniu do VPN, Admin Workstation ma dostęp do DMZ i może łączyć się z Giteą po SSH.

---

## Etap 2 – Konfiguracja kluczy SSH

Na **Admin Workstation** wygenerowano dedykowany klucz do Gitei:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_gitea -C "gitea-lab-key"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_gitea

Dodano wpis do ~/.ssh/config:
Host gitea-lab
    HostName 10.10.30.11
    Port 2222
    User .....
    IdentityFile ~/.ssh/id_rsa_gitea

Klucz publiczny (~/.ssh/id_rsa_gitea.pub) dodano w Gitei
→ Settings → SSH Keys → Add Key

---

## Etap 3 - Dodanie drugiego zdalnego repozytorium

Na Admin Workstation:
git remote add gitea ssh://....@10.10.30.11:2222/gitea/hybrid-lab.git

Sprawdzenie:
git remote -v

---

## Etap 4 – Workflow DevOps

1 Development	Tworzysz / edytujesz kod na Admin Workstation	git add . && git commit -m "update"
2 Push publiczny	Aktualizacja repo GitHub	git push origin main
3 Push wewnętrzny	Mirror repozytorium w Gitei (VPN)	git push gitea main
4 Deployment	Jump Host pobiera kod z Gitei i uruchamia automatyzację	git pull origin main && ansible-playbook site.yml

---
