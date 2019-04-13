---
draft: true
---
# Setting up my user account with sudo

useradd --create-home paul
passwd paul

visudo

paul  ALL=(ALL) ALL
