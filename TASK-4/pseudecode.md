// Üniversite Ders Kayıt Sistemi Pseudocode

YAPILANDIR
    Acılan_Dersler = LISTE (Ders_ID, Ad, Kontenjan, Kredi, Saat, Gun)
    Ogrenci_Bilgileri = LISTE (Ogrenci_ID, Transkript, Kayıtlı_Dersler, Danısman_ID)
    Kayıt_Donemi_Acık = BOOLEAN (Doğru/Yanlış)

PROSEDÜR Ders_Kaydı_Yap()
    EKRANA_YAZ "Öğrenci Girişi Yapınız (ID ve Şifre)."
    OgrID = KULLANICIDAN_AL

    EĞER Kayıt_Donemi_Acık = YANLIŞ İSE
        EKRANA_YAZ "Ders kayıt dönemi şu anda kapalıdır."
        BITIR
    END_EĞER

    EĞER Ogrenci_Dogrulandı(OgrID) İSE
        SEPET = BOS_LISTE
        Ders_Seçimi_Arayüzünü_Göster(Acılan_Dersler)

        DÖNGÜ Seçim_Tamamlanana_Kadar
            SecilenDersID = KULLANICIDAN_AL
            SecilenDers = Acılan_Dersler'den BUL(SecilenDersID)

            EĞER Kontrol_Yap(OgrID, SecilenDers, SEPET) İSE
                SEPET'e SecilenDers EKLE
                EKRANA_YAZ SecilenDers.Ad + " sepete eklendi. Kontenjan: " + (SecilenDers.Kontenjan - 1)
            DEĞİLSE
                EKRANA_YAZ "Ders eklenemedi (Çakışma, Kontenjan veya Önkoşul Hatası)."
            END_EĞER
        END_DÖNGÜ

        EKRANA_YAZ "Ders Seçimi Tamamlandı. Onay için Danışmana Gönderiliyor..."
        
        // Danışman Onayı Süreci
        DanismanOnayı = Danisman_Onayını_Bekle(OgrID, SEPET)
        
        EĞER DanismanOnayı = ONAYLANDI İSE
            Final_Dersleri = SEPET
            HER_BIR Ders Final_Dersleri İÇİN
                Ogrenci_Bilgileri'ni GUNCELLE(OgrID, Ders)
                Ders.Kontenjan = Ders.Kontenjan - 1
            END_HER_BIR
            EKRANA_YAZ "Ders Kaydınız başarıyla tamamlanmıştır."
        DEĞİLSE
            EKRANA_YAZ "Ders kaydınız danışman tarafından onaylanmadı. Lütfen danışmanınızla iletişime geçiniz."
        END_EĞER

    DEĞİLSE
        EKRANA_YAZ "Geçersiz Kullanıcı Adı veya Şifre."
    END_EĞER
END_PROSEDÜR

// Yardımcı Fonksiyon: Çakışma ve Kontenjan Kontrolü Yapar
FONKSİYON Kontrol_Yap(OgrID, Ders, Mevcut_Sepet) İade_Et BOOLEAN
    // 1. Kontenjan Kontrolü
    EĞER Ders.Kontenjan <= 0 İSE IADE_ET YANLIŞ
    
    // 2. Ders Çakışması Kontrolü
    HER_BIR SepetDers Mevcut_Sepet'te İÇİN
        EĞER Ders.Saat = SepetDers.Saat VE Ders.Gun = SepetDers.Gun İSE IADE_ET YANLIŞ // Zaman çakışması
    END_HER_BIR
    
    // 3. Önkoşul Kontrolü (Basitleştirilmiş)
    EĞER Onkosullar_Tamamlanmadı(OgrID, Ders) İSE IADE_ET YANLIŞ

    IADE_ET DOĞRU
END_FONKSIYON

// Ana Akış
BASLA
    Ders_Kaydı_Yap()
BITIR
