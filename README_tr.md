### Başlarken
   Bu bölümde server kullanırken ihtiyaç duyabileceğimiz püf noktalar, komut satırları ve uygulamalar hakkında konuşacağız. Görüş ve geri bildirimlerinizi  issues kısmından bildirirseniz, bana ve öğrenmek isteyenlere yardımcı olabilirsiniz.

### AWS Hizmeti İle Instance Oluşturma
Bu hizmeti kullanabilmek için öncelikle AWS konsolunuza giriş yapmalısınız. 

Konsolunuza giriş yaptıktan sonra **Services** sekmesinin altından **Compute** sekmesine ve Compute sekmesi altından **EC2** sekmesine giriyoruz. Bu sekmenin içerisinden **Launch instance** sekmesine geçiş yapıyoruz. Yani yolumuz şu şekilde:
> Services->Compute->EC2->Launch instance

Artık bu sekme üzerinden kendimize bir instance yani bir makine oluşturabiliriz.  Burada oluşturulması gösterilen makine **Ubuntu 18.4** içermekte ve ilerleyen safhalarda Ubuntu'nun bu sürümüne yönelik yönergeler yer almaktadır. Şimdi aşağıdaki adımları izleyelim:

**!** Buradaki adımlar, AWS'nin ücretsiz sağladığı hizmetler çerçevesinde şekillenmiştir.  İşletim sistemi dışındaki seçenekler ihtiyaca göre şekillendirilebilir.

1. Öncelikle isimlendirmemizi yapıyoruz:

[![](/images/names_and_tags.png)](/images/names_and_tags.png)


2.  Makinemizin işletim sistemini seçiyoruz:

[![](/images/machine_image.png)](/images/machine_image.png)


3. Instance typemızı seçiyoruz:

[![](/images/instance_type.png)](/images/instance_type.png)


4. AWS üzerinde root kullanıcımız için direkt şifre oluşturamıyoruz, bu nedenle makinemize bağlabilmek için bir key pair almamız gerekiyor:


[![](/images/key_pair.png)](/images/key_pair.png)


5. Key pairi oluştururken dikkat etmemiz gereken noktalardan bir tanesi .ppk uzantılı şekilde oluşturmaktır. Eğer key pairinizi .ppk uzantılı oluşturmadıysanız bir sonraki bölümde .pem uzantısından .ppk uzantısına dönüşüm için gerekli adımlar belirtilecektir. (Resimde yanlış belirtilmiş, düzelteceğim.)

[![](/images/create_key_pair.png)](/images/create_key_pair.png)


6. Eğer Summary bölümünüz aşağıda bulunan resimdeki gibiyse artık instancemizi başlatabiliriz:

[![](/images/launch_instance.png)](/images/launch_instance.png)

#### PuTTy ile key pair dönüştürme:
Aşağıdaki adımları izleyerek .pem uzantılı key pairimizi .ppk uzantılı hale çevireceğiz. Bu adımlara geçmeden  önce PuTTy edinmeniz gerekmektedir:

1. PuTTygen'e giriş yapalım


2. Load sekmesi ile AWS'den edindiğimiz .pem uzantılı keyimizi açıyoruz ve generate işlemi ile .ppk uzantılı dosyaya çeviriyoruz:

[![](/images/convert_key.png)](/images/convert_key.png)


3. Save public key diyerek keyimizi bildiğimiz bir konuma kaydedelim.

#### PuTTy ile bağlanma:
Aşağıdaki adımları izleyerek .ppk uzantılı keyimizi kullanarak serverimize gireceğiz, key ile login işlemini kaldıracağız ve password ile logine izin vereceğiz ardından root için şifre oluşturacağız:


1.  Ip adresimizi bu bölüme yazalım:

[![](/images/convert_key.png)


2.  **Connection/SSH/Auth** sekmesine geçiş yapalım ve keyimizi indirdiğimiz konumu seçelim ardından open ile onaylayalım:

[![](/images/select_key_file.png)](/images/select_key_file.png)


3.  Açılan terminalde aşağıdaki gibi devam edelim:
`login as: ubuntu`

4.  `sudo nano /etc/ssh/sshd_config  `
komutu ile ssh_config dosyasını düzenleme işlemini başlatalım.

5.  Düzenlemeye açılan sshd_config dosyasının 
`PasswordAuthentication yes`
haline getirelim. 
Ardından boş herhangi bir satıra :
`PermitRootLogin yes`
izinini ekleyelim.

6. Ctrl + X ile düzenleme işlemini kaydedelim.

7.  Aşağıdaki komut ile root için şifre oluşturalım:
`sudo passwd root`

8.  İzinleri değiştirdiğimiz için 
`sudo service sshd restart  `
veya 
`sudo systemctl restart sshd`
komutlarınan  bir tanesiyle sshd hizmetimizi yeniden başlatıyoruz.

9. Aşağıdaki komutla sistem güncellemelerini indirelim. Ayriyetten bu işlemi her giriş yaptığımızda yapmaya çalışalım:
`sudo apt update && sudo apt upgrade -y`

**!** Buradan sonraki adımlara, kullanıcı eklemek isterseniz devam edebilirsiniz.

10. Aşağıdaki komutla user-name isimli bir user oluşturalım. 
`sudo adduser user-name`

11. Aşağıdaki komutla isterseniz ilgili kullanıcıya root yetkilerini kullanmasına izin verebilirsiniz:
`sudo usermod -aG sudo user-name`

12. Bu komutla user-name adlı kullanıcıya şifre belirleyelim:
`sudo passwd user-name`

Bu adımlar ile şifre ile erişim için makinemizin ayarlarını düzenledik. Artık Windows için herhangi bir SSH clienti kullanarak, MacOS veya Linux için kendi konsolunuza  `ssh root@ip-adsress` yazarak bağlantı kurabilirsiniz. 

### Bash Scripting:

* Bash betikleri olan dosyalara .sh uzantısı (örneğin myscript.sh) vermek gelenekseldir. 

:warning: Linux uzantısız bir sistemdir, dolayısıyla bir betiğin çalışması için .sh uzantısına sahip olması gerekmez.

* Bir betiği çalıştırabilmek için yürütme izni verilmesi gerekir (güvenlik nedeniyle bu izin genellikle varsayılan olarak ayarlanmaz). Komut dosyasını çalıştırmadan önce yürütme iznini vermeyi unutursanız, aşağıdaki hatayı alırsınız:

```bash
user@bash: ./myscript.sh
bash: ./myscript.sh: Permission denied
```

Yürütme iznini vermek için `chmod 755 myscript.sh` komutu kullanılır.

:warning: Bir script çalıştırılırken `./myscript.sh` veya `/path/to/script/myscript.sh` şeklinde çalıştırılmalıdır. Bu çalıştırma biçimiyle yalnızca belirtilen konumdaki scripti çalıştırabilirsiniz. Eğer herhangi bir konumdan scriptinize direkt erişim yapmak isterseniz, bu scriptin bulunduğu konumu $PATH'e eklemeniz gerekir.

* Bir script yazılırken başlangıcına bu kümenin bir script olduğunu belirten -aynı HTML'de olduğu gibi- bir karakter ve format dizisi eklenir.Bu format dizisine Shebang adı verilir. Shebang `#!/bin/bash` şeklindedir.

:warning: Shebang her zaman ilk satırda bulunmalıdır. Boş bir satır dahi bırakılmamalıdır. 

:warning: Shebang genel anlamda interpreterin konumunu belirtir. Bu nedenle farklı konum bildirme biçimleri kullanılabilir. Ancak en garanti yöntem mutlak konum bildirme olduğundan en çok bu yöntem tercih edilir. 




### Kaynaklar:

* https://ryanstutorials.net/bash-scripting-tutorial/