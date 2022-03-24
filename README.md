class Sirket():
    def __init__(self,ad):
        self.ad = ad
        self.calisma = True

    def program(self):
        secim=self.menuSecim()

        if secim == 1:
            self.calisanEkle()
        if secim == 2:
            self.calisanCikar()
        if secim == 3:
            ay_yil_secim=input("yillik bazda görmek isterseniz?(e/h)")
            if ay_yil_secim=="e":
                self.verilecekMaasGoster(hesap="y")
            else:
                self.verilecekMaasGoster()
        if secim == 4:
            self.maaslariVer()
        if secim == 5:
            self.masrafGir()
        if secim == 6:
            self.gelirGir()
    def menuSecim(self):
        secim = int(input("---- {} Otomasyona Hoş Geldiniz ----\n\n1-Çalışan Ekle\n2-Calışan Çıkar\n3-Verilecek Maaşları Göster\n4-Maaşları Ver\n5-Masraf Gir\n6-Gelir gir\nSeçiminizi giriniz:".format(self.ad)))
        while secim<1 or secim>6:
            secim = int(input("Lütfen 1-6 arasında belirtilen seçeneklerden birini giriniz: "))
        return secim

    def calisanEkle(self):
        id=1
        ad = input("Çalışanın adını giriniz:")
        soyad = input("Çalışanın soyadını giriniz:")
        yas = input("Çalışanın yaşını giriniz:")
        cinsiyet = input("Çalışanın cinsiyetini giriniz:")
        maas = input("Çalışanın maasşını giriniz:")

        with open("calisanlar.txt","r") as dosya:
            calisanListesi = dosya.readlines()


        if len(calisanListesi) == 0:
            id=1
        else:
            with open("calisanlar.txt", "r") as dosya:
                id=int(dosya.readlines()[-1].split(")")[0])+1

        with open("calisanlar.txt","a+") as dosya:
            dosya.write("{}){}-{}-{}-{}-{}\n".format(id,ad,soyad,yas,cinsiyet,maas))



    def calisanCikar(self):
        with open("calisanlar.txt","r") as dosya:
            calisanlar = dosya.readlines()
        gCalisanlar=[]

        for calisan in calisanlar:
            gCalisanlar.append(" ".join(calisan[:-2].split("-")))
        for calisan in gCalisanlar:
            print(calisan)

        secim = int(input("lütfen çıkarmak istediğiniz çalışanın numarasını giriniz(1-{}:)".format(len(gCalisanlar))))
        while secim<1 or secim>len(gCalisanlar):
            secim = int(input("lütfen (1-{}) arasında numara giriniz:".format(len(gCalisanlar))))
        calisanlar.pop(secim-1)
        sayac=1
        dCalisanlar=[]
        for calisan in calisanlar:
            dCalisanlar.append(str(sayac) + ")" + calisan.split(")")[1])
            sayac +=1
        with open("calisanlar.txt","w") as dosya:
            dosya.writelines(dCalisanlar)


    def verilecekMaasGoster(self,hesap = "a"):
        #hesap:a ise aylık, y ise yıllak hesap
        with open("calisanlar.txt","r") as dosya:
            calisanlar=dosya.readlines()
        maaslar=[]

        for calisan in calisanlar:
            maaslar.append(int(calisan.split("-")[-1]))
        if hesap=="a":
            print("Bu ay verilmesi gereken maaş:{}".format(sum(maaslar)))
        else:
            print("Bu ay verilmesi gereken maaş:{}".format(sum(maaslar)*12))
    def maaslariVer(self):
        with open("calisanlar.txt","r") as dosya:
            calisanlar=dosya.readlines()
        maaslar=[]

        for calisan in calisanlar:
            maaslar.append(int(calisan.split("-")[-1]))
        toplamMaas = sum(maaslar)
       #bütçeden maası alma
        with open("butce.txt","r") as dosya:
            tbutce=int(dosya.readlines()[0])
        tbutce = tbutce - toplamMaas

        with open("butce.txt", "w") as dosya:
            dosya.write(str(tbutce))
    def masrafGir(self):
        pass

    def gelirGir(self):
        pass

sirket = Sirket("Paradise Energy")

while sirket.calisma:
    sirket.program()

