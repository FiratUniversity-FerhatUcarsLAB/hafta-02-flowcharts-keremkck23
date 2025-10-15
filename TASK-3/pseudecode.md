// Hastane Randevu Sistemi Pseudocode

YAPILANDIR
    Randevu_Veritabani = LISTE (Randevu_ID, Hasta_ID, Doktor_ID, Tarih, Saat, Durum)
    Doktor_Takvimi = LISTE (Doktor_ID, Musait_Zaman_Slotları)

PROSEDÜR Randevu_Al()
    EKRANA_YAZ "Randevu Almak İçin Kimlik No Giriniz."
    HastaID = KULLANICIDAN_AL
    EĞER Hasta_Kaydı_Mevcut(HastaID) İSE
        POLIKLINIK_SEC = KULLANICIDAN_AL
        DOKTOR_LISTESI_GOSTER(POLIKLINLIK_SEC)
        DoktorID = KULLANICIDAN_AL
        
        MUSAIIT_SLOTLARI_GOSTER(DoktorID)
        SecilenTarihSaat = KULLANICIDAN_AL

        EĞER Slot_Musait_mi(DoktorID, SecilenTarihSaat) İSE
            // Randevu kaydını oluştur
            YENI_RANDEVU = YENI_KAYIT(HastaID, DoktorID, SecilenTarihSaat, "Onaylandı")
            Randevu_Veritabani'na YENI_RANDEVU EKLE
            Doktor_Takvimi'ni GUNCELLE(DoktorID, SecilenTarihSaat, "Dolu")
            EKRANA_YAZ "Randevunuz " + SecilenTarihSaat + " için başarıyla oluşturuldu."
            SMS_GONDER(HastaID, "Randevu Onayı")
        DEĞİLSE
            EKRANA_YAZ "Seçilen slot artık müsait değil veya geçersizdir."
        END_EĞER
    DEĞİLSE
        EKRANA_YAZ "Geçerli bir hasta kaydı bulunamadı. Lütfen önce kayıt olunuz."
    END_EĞER
END_PROSEDÜR

PROSEDÜR Randevu_Iptal_Et(RandevuID)
    BUL Randevu Randevu_Veritabani'nda (RandevuID'ye göre)
    EĞER Randevu BULUNDU İSE
        Randevu.Durum = "İptal Edildi"
        Doktor_Takvimi'ni GUNCELLE(Randevu.Doktor_ID, Randevu.Tarih_Saat, "Müsait")
        EKRANA_YAZ "Randevunuz başarıyla iptal edilmiştir."
        SMS_GONDER(Randevu.Hasta_ID, "Randevu İptali")
    DEĞİLSE
        EKRANA_YAZ "Geçersiz Randevu Numarası."
    END_EĞER
END_PROSEDÜR

// Ana Akış Başlangıcı
BASLA
    DÖNGÜ SÜREKLİ
        EKRANA_YAZ "1: Randevu Al | 2: Randevu İptal Et | 3: Çıkış"
        SECIM = KULLANICIDAN_AL
        
        EĞER SECIM = 1 İSE
            Randevu_Al()
        YOKSA EĞER SECIM = 2 İSE
            EKRANA_YAZ "İptal etmek istediğiniz Randevu ID'yi giriniz."
            RandevuID_Iptal = KULLANICIDAN_AL
            Randevu_Iptal_Et(RandevuID_Iptal)
        YOKSA EĞER SECIM = 3 İSE
            DÖNGÜ_SONU
        DEĞİLSE
            EKRANA_YAZ "Geçersiz Seçim."
        END_EĞER
    END_DÖNGÜ
BITIR
