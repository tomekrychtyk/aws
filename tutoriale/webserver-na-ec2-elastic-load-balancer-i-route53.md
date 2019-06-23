---
description: 'Koszt: ok. $0.5 / miesiąc (AWS free tier)'
---

# WebServer na EC2, Elastic Load Balancer i Route53

### Wprowadzenie

W tym mini-tutorialu pokażę, jak postawić w pełni funkcjonalny serwer WWW \(**Apache**\) przy użyciu **EC2** i **load balancera** oraz jak skonfigurować **Route53**, aby osługowało domenę zarejestrowaną u zewnętrznego dostawcy \(w tym przykładzie będzie do portal [domeny.pl](https://domeny.pl)\).

### Odpalamy EC2

Pierwszym krokiem będzie oczywiście uruchomienie działającej instancji. W tym celu logujemy się do konsoli AWS i wybieramy **EC2** z grupy serwisów **Compute**. Otworzy nam się panel do zarządzania instancjami EC2. Tutaj klikamy na przycisk **Launch Instance** i wybieramy jeden z systemów operacyjnych, na jakim będzie stał serwer. Ja na potrzeby tego tutoriala wybrałem **Ubuntu Server 18.04 LTS**. Po dokonaniu wyboru systemu musimy jeszcze wybrać typ instancji. Będąc na _free tier_ mamy do wyboru tylko jeden typ jakim jest **t2.micro**. Oczywiście możemy wybrać również inny typ instancji, ale wówczas nie będzie on kwalifikował się do _free tier_, co będzie wiązać się z naliczeniem odpowiedniej opłaty w zależności od wybranego typu. Dlatego też myślę, że na potrzeby tutoriala t2.micro w zupełności wystarczy.  
Klikamy na **Next: Configure Instance Details**, zostawiamy wszystkie pola z domyślnymi wartościami i przechodzimy dalej do **Next: Add Storage**. Tutaj również pozostawiamy wartości domyślne i klikamy na **Next: Add Tags**. Możemy otagować tworzoną instancję, np. nadać jej jakąś własną nazwę. W tym celu klikamy na **Add Tags**, w polu **Key** wpisujemy np. "Name", a w polu **Value** "Webserver" \(lub jakąkolwiek inną nazwę\). Następnym krokiem jest **Next: Configure Security Groups**. Tutaj możemy zdefiniować dostęp do naszej instancji. W polu **Security group name** możemy podać nazwę nasej grupy np. "Webserver". Domyślnie otwarty będzie port 22 do dostępu po SSH. Jako że chcemy postawić w pełni sprwny serwer WWW, musimy także otworzyć porty **80** i **443**, aby umożliwić dostęp odpowiednio po HTTP i HTTPS. W tym celu klikamy na **Add Rule** i z listy wybieramy HTTP, a następnie czynność tę powtarzamy dla protokołu HTTPS.  
Teraz możemy kliknąć w **Review and launch**, a następnie **Launch**. Pojawi nam się okienko, w którym będziemy musieli utworzyć, lub wybrać aktualną parę kluczy, która będzie użyta do łączenia się z instancją przez SSH. Wybieramy z listy **Create a new key pair**, wpisujemy jej nazwę, np. "Webserver Keypair" i klikamy na przycisk **Download Key Pair**. Zapisujemy plik w wybranej przez nas lokalizacji, jednak należy oczywiście pamiętać gdzie go zapisujemy, bo inaczej nie połączymy się z instancją. Teraz wreszcie możemy kliknąć **Launch Instances** i proces tworzenia instancji zostanie rozpoczęty. Zwykle trwa on kilka minut.

{% hint style="warning" %}
Jeśli przy próbie utworzenia nowej instancji EC2 pokazuje się błąd "Launch failed" i opis błędu mówi coś o weryfikacji konta/requestu - nie przejmuj się, tak czasem się może zdarzyć przy nowych kontach. Z reguły po kilku - kilkunastu minutach powinieneś trzymać maila od Amazonu, że wszystko jest w porządku i twoje konto jest w pełni zwerfikowane.  
Jednakże jeśli w ciągu 4 godzin nie dostaniesz maila, warto napisać do supportu i zapytać jaki jest status Twojego konta.
{% endhint %}

#### Instalacja Apache

