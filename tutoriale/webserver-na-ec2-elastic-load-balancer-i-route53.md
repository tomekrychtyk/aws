---
description: 'Koszt: ok. $0.5 / miesiąc (AWS free tier)'
---

# WebServer na EC2, Elastic Load Balancer i Route53

### Wprowadzenie

W tym mini-tutorialu pokażę, jak postawić w pełni funkcjonalny serwer WWW \(**Apache**\) przy użyciu **EC2** i **load balancera** oraz jak skonfigurować **Route53**, aby osługowało domenę zarejestrowaną u zewnętrznego dostawcy \(w tym przykładzie będzie do portal [domeny.pl](https://domeny.pl)\).

### Odpalamy EC2

Pierwszym krokiem będzie oczywiście uruchomienie działającej instancji. W tym celu logujemy się do konsoli AWS i wybieramy **EC2** z grupy serwisów **Compute**. Otworzy nam się panel do zarządzania instancjami EC2. Tutaj klikamy na przycisk **Launch Instance** i wybieramy jeden z systemów operacyjnych, na jakim będzie stał serwer. Ja na potrzeby tego tutoriala wybrałem **Ubuntu Server 18.04 LTS**. Po dokonaniu wyboru systemu musimy jeszcze wybrać typ instancji. Będąc na _free tier_ mamy do wyboru tylko jeden typ jakim jest **t2.micro**. Oczywiście możemy wybrać również inny typ instancji, ale wówczas nie będzie on kwalifikował się do _free tier_, co będzie wiązać się z naliczeniem odpowiedniej opłaty w zależności od wybranego typu. Dlatego też myślę, że na potrzeby tutoriala t2.micro w zupełności wystarczy.  
Klikamy na **Next: Configure Instance Details**, zostawiamy wszystkie pola z domyślnymi wartościami i przechodzimy dalej do **Next: Add Storage**. Tutaj również pozostawiamy wartości domyślne i klikamy na **Next: Add Tags**. Możemy otagować tworzoną instancję, np. nadać jej jakąś własną nazwę. W tym celu klikamy na **Add Tags**, w polu **Key** wpisujemy np. "Name", a w polu **Value** "Webserver" \(lub jakąkolwiek inną nazwę\). Następnym krokiem jest Next: Configure Security Groups. Tutaj możemy zdefiniować dostęp do naszej instancji. W polu Security group name możemy podać nazwę nasej grupy np. "Webserver". Domyślnie otwarty będzie port 22 do dostępu po SSH. Jako że chcemy postawić w pełni sprwny serwer WWW, musimy także otworzyć porty 80 i 443, aby umożliwić dostęp odpowiednio po HTTP i HTTPS. W tym celu klikamy na Add Rule i z listy wybieramy HTTP, a następnie czynność tę powtarzamy dla protokołu HTTPS.

