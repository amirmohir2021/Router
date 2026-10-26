1-dars: Static Routing







cisco.com


Avval qisqacha tushuncha:
- Static Route — administrator routerga qaysi tarmoqqa qaysi yo‘l orqali borishni qo‘lda ko‘rsatadi.
- Default Route — routerda aniqroq yo‘l topilmasa, trafikni belgilangan keyingi routerga yuboradi.
- Floating Static Route — asosiy yo‘l ishlamay qolsa, zaxira yo‘l sifatida ishlatiladi.
Bularni uchta alohida nazariy dars qilib cho‘zmaymiz. Bitta kichik laboratoriyada bosqichma-bosqich o‘rganamiz.
Birinchi ishimiz
Yangi, alohida Packet Tracer fayli ochamiz. Oldingi enterprise loyihamizni o‘zgartirmaymiz.
Laboratoriya topologiyasi
PC1

192.168.1.10/24

R1

192.168.1.1

R2

192.168.12.2


R1 va R2 orasidagi aloqa: 192.168.12.0/30. R2 ortida ikkinchi LAN tarmog‘i bo‘ladi.

Hozir faqat birinchi qadam: Packet Tracer’da yangi fayl ochib, ikkita router va ikkita switch hamda ikkita PC joylashtir. Qurilmalarni PC1 → SW1 → R1 → R2 → SW2 → PC2 tartibida ulang.

Tayyor bo‘lgach, “tayyor” deb yoz. Keyin portlarni aniqlab, IP manzillarni birma-bir sozlaymiz.
















tayyor












Yaxshi, boshlaymiz! Bu laboratoriyada Static Route, Default Route va Floating Static Route’ni amalda o‘rganamiz. Oldingi enterprise loyihamizga tegmaymiz.
1-qadam: Qurilmalarni ulash va IP manzillarni rejalashtirish
Avval topologiyamizni aniq belgilaymiz. Hozircha faqat IP manzillarni sozlaymiz, routing buyruqlariga keyin o‘tamiz.
Bizning topologiyamiz
PC1 ── SW1 ── R1 ═════ R2 ── SW2 ── PC2

Quyidagi IP manzillar rejasidan foydalanamiz:
Qurilma	Interfeys	IP manzil	Subnet mask
PC1	FastEthernet0	192.168.1.10	255.255.255.0
R1	LAN interfeysi	192.168.1.1	255.255.255.0
R1	R2 tomonga	192.168.12.1	255.255.255.252
R2	R1 tomonga	192.168.12.2	255.255.255.252
R2	LAN interfeysi	192.168.2.1	255.255.255.0
PC2	FastEthernet0	192.168.2.10	255.255.255.0


PC’larning Default Gateway manzillari:
- PC1: 192.168.1.1
- PC2: 192.168.2.1
Nima uchun routerlar orasida /30 ishlatyapmiz?
192.168.12.0/30 tarmog‘ida jami 4 ta IP manzil mavjud:
- 192.168.12.0 — Network address
- 192.168.12.1 — R1 uchun
- 192.168.12.2 — R2 uchun
- 192.168.12.3 — Broadcast address
Routerlar orasidagi nuqtadan nuqtaga ulanish uchun ikkita foydalaniladigan IP manzil yetarli.
Hozirgi vazifa
1. Jadvaldagi IP manzillarni PC1 va PC2 ga kiriting.
2. Default Gateway’larni ham belgilang.
3. Routerlarning interfeyslarini hozircha sozlamang — keyingi qadamda ularni aniq portlar bo‘yicha sozlaymiz.
Tugatganingda tayyor deb yoz. Keyin R1 va R2 interfeyslarini sozlaymiz va ular orasidagi aloqani tekshiramiz.













tayyor












Yaxshi! Endi routerlarning interfeyslarini sozlaymiz. Hozircha faqat R1 routerini sozlaymiz, keyin natijasini tekshiramiz.
2-qadam: R1 routerini sozlash
Avval R1 qurilmasini ochib, CLI bo‘limiga kiring.

Quyidagi buyruqlarni kiriting:
enable
configure terminal
interface gigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet0/1
ip address 192.168.12.1 255.255.255.252
no shutdown
end

Buyruqlar nima qiladi?
- interface gigabitEthernet0/0 — PC1 joylashgan LAN tomonidagi interfeysni sozlaydi.
- ip address 192.168.1.1 255.255.255.0 — R1 ning LAN manzilini belgilaydi.
- interface gigabitEthernet0/1 — R2 tomonidagi interfeysni sozlaydi.
- ip address 192.168.12.1 255.255.255.252 — routerlar orasidagi ulanish manzilini belgilaydi.
- no shutdown — interfeysni yoqadi.
Muhim: Agar sening routeringda GigabitEthernet0/0 va GigabitEthernet0/1 portlari bo‘lmasa, boshqa interfeys nomlarini taxmin qilib ishlatma. Avval menga port nomlarini yubor.
Tekshirish
Buyruqlar tugagach, R1 da quyidagini bajar:
show ip interface brief

Biz ikkala interfeysda ham up/up holatini ko‘rishni xohlaymiz. Agar down/down yoki up/down chiqsa, bu ulanish yoki interfeys holatini tekshirish kerakligini bildiradi.

Natijani yubor. Keyin R2 ni sozlaymiz.













Router#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.1.1     YES manual up                    up 
GigabitEthernet0/1     192.168.12.1    YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down












Ajoyib! ✅ R1 routerining ikkala interfeysi ham up/up holatida. Demak, interfeyslar ishlayapti.

Endi R2 routerini sozlaymiz.
3-qadam: R2 routerini sozlash
Packet Tracer’da R2 → CLI oynasini oching va quyidagi buyruqlarni kiriting:
enable
configure terminal
interface gigabitEthernet0/0
ip address 192.168.12.2 255.255.255.252
no shutdown
exit
interface gigabitEthernet0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
end

Nima uchun bu manzillarni berdik?
- G0/0 — 192.168.12.2/30: R1 bilan aloqa qilish uchun.
- G0/1 — 192.168.2.1/24: PC2 joylashgan LAN uchun.
Eslab qol: R1 va R2 orasidagi tarmoq 192.168.12.0/30. R1 manzili .1, R2 manzili .2.
Tekshirish
R2 da quyidagi buyruqni bajar:
show ip interface brief

Ikkala interfeys ham up/up bo‘lishi kerak.

Natijani yubor. Keyin routerlar orasidagi aloqani ping orqali tekshiramiz. Shundan so‘ng Static Route sozlashni boshlaymiz.













Router#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.12.1    YES manual up                    up 
GigabitEthernet0/1     192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down












Yaxshi, interfeyslar up/up, lekin bitta muhim nomuvofiqlik bor: sen yuborgan natijada R2 ning GigabitEthernet0/0 manzili 192.168.12.1 bo‘lib turibdi. Bu esa R1 ga berilgan manzil edi.

Bizning rejamiz bo‘yicha:
- R1 G0/1: 192.168.12.1/30
- R2 G0/0: 192.168.12.2/30
Agar ikkala routerda ham .1 bo‘lsa, IP manzil to‘qnashuvi yuz beradi. Buni hozir tuzatamiz.
4-qadam: R2 IP manzilini to‘g‘rilash
R2 routerida quyidagilarni kiriting:
enable
configure terminal
interface gigabitEthernet0/0
ip address 192.168.12.2 255.255.255.252
end

Keyin tekshiring:
show ip interface brief

GigabitEthernet0/0 qatorida 192.168.12.2 ko‘rinishi kerak.

Eslatma: Bu bosqichda boshqa konfiguratsiyalarni o‘zgartirmang. Natijani yuboring, keyin R1 va R2 o‘rtasida ping sinovini bajaramiz.













Router#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.12.2    YES manual up                    up 
GigabitEthernet0/1     192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down












Ajoyib! ✅ Endi R1 va R2 orasidagi IP manzillar to‘g‘ri:
- R1 G0/1: 192.168.12.1/30
- R2 G0/0: 192.168.12.2/30
Ikkala interfeys ham up/up. Endi routerlar bir-biriga IP orqali yetib bora olishini tekshiramiz.
5-qadam: Routerlar orasida ping
R2 router CLI oynasida quyidagi buyruqni bajaring:
ping 192.168.12.1

Bu R2 dan R1 ning routerlararo interfeysiga ping yuboradi.

Kutiladigan natija: !!!!! va 100 foiz muvaffaqiyat.


Router#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0
S    192.168.2.0/24 [1/0] via 192.168.12.2
     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.12.0/30 is directly connected, GigabitEthernet0/1
L       192.168.12.1/32 is directly connected, GigabitEthernet0/1
