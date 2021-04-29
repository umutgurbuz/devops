# devops

Trendyol System Bootcamp Mezuniyet Projesi 1
Case 1


Öncelikle VirtualBox kurdum ve Centos7 imajını indirdim. Ardından, VirtualBox üzerine 32GB’lık bir disk alanı ile Centos7’yi kurdum.


Aşağıdaki komutları kullanarak umut.gurbuz adında yeni bir kullanıcı oluşturdum ve ona şifre ile sudo yetkileri verdim ve çıkış yaptım.
sudo adduser umut.gurbuz
sudo passwd umut.gurbuz
gpasswd -a umut.gurbuz wheels


VirtualBox→Centos7 Settings→Storage→Add Harddisk yolunu takip ederek 10GB’lık yeni bir disk ekledim.
-umut.gurbuz kullanıcısı ile tekrar giriş yaptım. “lsblk” komutunu çalıştırdığımda 10 GB alanım sdb olarak eklenmiş görünüyordu ancak mountpoint’i ya da partition’ı yoktu. Aşağıdaki komutla fdisk partition toolunu kullanarak yeni eklediğim diskte 5 GB’lık bir partition oluşturdum.
sudo fdisk /dev/sdb


-Tekrar “lsblk” ile baktığımda 5GB’lık partition sdbnin altında sdb1 olarak görünmeye başladı.
Ardından aşağıdaki komutlarla diskimi mount edeceğim dizini oluşturdum, ext4 dosya sistemi yarattım ve diski dizine mount ettim.
`sudo mkdir -p  /opt/mntpoint`

`sudo mkfs.ext4 /dev/sdb1`

`sudo mount /dev/sdb1 /opt/mntpoint`
-Makinamı yeniden başlattığımda da mount’un kalıcı olması için /etc/fstab altına aşağıdaki satırı ekledim.
`/dev/sdb1               /opt/mntpoint           ext4    defaults        0 0`
-Ardından aşağıdaki komutu kullanarak /opt/bootcamp altında bootcamp.txt isimli bir dosya oluşturdum ve içine “merhaba trendyol” yazdım.
`sudo echo merhaba trendyol>>/opt/bootcamp/bootcamp.txt`
-Ardından kendi home dizinime geçtim ve aşağıdaki komutu kullanarak bootcamp.txt dosyasını /opt/mntpoint altına taşıdım. Bu komut root dizininin altındaki her dizini bootcamp.txt isimli bir dosya bulana kadar tarıyor ve bulduğunda /opt/mntpoint dizininin içine atıyor.
`sudo find / -name bootcamp.txt -exec mv -t /opt/mntpoint/. {} +`
Case 2
İkinci case için ubuntu hostuma ansible kurdum ve virtualboxta kurulu olan centos7de githubdan çektiğim uygulamayı dockerize etmeye çalıştım. Öncelikle /etc/ansible/hosts dosyasına centos7 makinemi ekledim. Ardından yazdığım playbook ile sırasıyla aşağıdaki işlemleri başarıyla yaptım. Bu işlemler sırasında centos7’yi sadece kurulumları kontrol etmek için kullandım.

Docker kurulumu
Docker-compose kurulumu
Python için Docker Yazılım Geliştirme Kiti kurulumu
Nginx kurulumu
Git kurulumu
Node.js kurulumu
Githubdan mongodb-nodejs uygulamasının çekilmesi
Node.js uygulamasının kullanacağı package dependency’lerin kurulumu

Not: Yazdığım playbookta path’i veriş şeklimin yanlış olduğunu ve eğer daha fazla nodeda çalıştırsaydım tüm makinelerde aynı işlemlerin gerçekleşebilmesi için generic bir path vermem gerektiğini biliyorum. Ancak ansible_env.HOME şeklinde denedim ve başarılı olmadı. Doğru kullanımı bulamadığım için bu seferlik playbooktaki pathi verdim.
Bundan bir sonraki aşamada docker-compose kullanarak uygulamayı dockerize edip çalıştıracaktım ama aşağıdaki hatayı almaya başladım. Hata muhtemelen ansible_python_interpreter’da set edilen python sürümünden kaynaklı ancak çok farklı sürümler denesem de sonuca ulaşamadım. Aynı zamanda araştırmalarım sonucu Python için Docker Yazılım Geliştirme Kiti kurulumunu farklı pip sürümleri kullanarak tekrar yaptım, ya da Centos7’nin default python’ını değiştirdim ancak bunlar da sorunu düzeltmedi. Bu sebeple uygulamamı dockerize edemedim ve sürece devam edemedim.
Muhtemelen gitten bir uygulama kodu çekmek yerine kendim çok daha basit bir uygulama ve bir dockerfile yazsaydım bu sorunlarla karşılaşmayacaktım.s kullanarak bootcamp=devops header’ı içeren requestleri Hoşgeldin Devops sayfasına yönlendirmeye çalışacaktım.
