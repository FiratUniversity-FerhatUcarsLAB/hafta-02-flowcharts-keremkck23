// Akıllı Ev Güvenlik Sistemi Pseudocode

YAPILANDIR
    Sistem_Durumu = "DEVRE_DIŞI" // Olası değerler: DEVRE_DIŞI, EVDE, UZAKTA
    Sensor_Verileri = LISTE (Tip, Durum, Konum)
    Kullanıcı_Listesi = LISTE (ID, Telefon, Acil_Durum_Kontak)

PROSEDÜR Sistemi_Kur(Yeni_Durum)
    EĞER Yeni_Durum = "UZAKTA" VEYA Yeni_Durum = "EVDE" İSE
        Sistem_Durumu = Yeni_Durum
        EKRANA_YAZ "Güvenlik Sistemi Durumu: " + Sistem_Durumu
    DEĞİLSE
        Sistem_Durumu = "DEVRE_DIŞI"
        EKRANA_YAZ "Güvenlik Sistemi Devre Dışı Bırakıldı."
    END_EĞER
END_PROSEDÜR

PROSEDÜR Sensor_Verilerini_Izle()
    DÖNGÜ SÜREKLİ
        HER_BIR Sensor Sensor_Verileri'nde İÇİN
            YENI_DURUM = Sensor.Durumu_Oku()

            EĞER YENI_DURUM = "İHLAL" İSE
                EĞER Sistem_Durumu = "UZAKTA" VEYA (Sistem_Durumu = "EVDE" VE Sensor.Tip = "DUMAN") İSE
                    EKRANA_YAZ "!!! İHLAL TESPİT EDİLDİ: " + Sensor.Tip + " " + Sensor.Konum
                    Alarm_Tepkisini_Baslat(Sensor.Tip, Sensor.Konum)
                END_EĞER
            END_EĞER

            BEKLE(1 Saniye) // Veri okuma sıklığı
        END_HER_BIR
    END_DÖNGÜ
END_PROSEDÜR

PROSEDÜR Alarm_Tepkisini_Baslat(Ihlal_Tipi, Konum)
    ALARM_SESINI_AC()
    KAMERA_KAYDINI_BASLAT(Konum)

    HER_BIR Kullanıcı Kullanıcı_Listesi'nde İÇİN
        BILDIRIM_GONDER(Kullanıcı.Telefon, Ihlal_Tipi + " ihlali tespit edildi! Konum: " + Konum)
    END_HER_BIR

    EĞER Ihlal_Tipi = "HIRSIZLIK" VEYA Ihlal_Tipi = "
