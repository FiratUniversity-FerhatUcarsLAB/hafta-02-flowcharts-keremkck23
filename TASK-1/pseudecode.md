// ATM Para Çekme Sistemi Pseudocode

BASLA

    EKRANA_YAZ "Lütfen kartınızı takınız."
    KART_OKU

    EĞER Kart_Gecerli İSE
        EKRANA_YAZ "Lütfen 4 haneli PIN kodunuzu giriniz."
        PIN_GIRIS = KULLANICIDAN_AL

        DÖNGÜ 3 KEZ (PIN_DENEME <= 3)
            EĞER PIN_GIRIS = Hesabın_Dogru_PIN_Kodu İSE
                DÖNGÜ_SONU // PIN doğru, döngüden çık
            DEĞİLSE
                PIN_DENEME = PIN_DENEME + 1
                EĞER PIN_DENEME < 3 İSE
                    EKRANA_YAZ "Yanlış PIN. Lütfen tekrar deneyiniz. Kalan deneme: " + (3 - PIN_DENEME)
                    PIN_GIRIS = KULLANICIDAN_AL
                DEĞİLSE
                    EKRANA_YAZ "PIN 3 kez yanlış girildi. Kartınıza el konulmuştur. Lütfen bankanızla iletişime geçiniz."
                    KARTI_ALIKOY
                    BITIR // Programı bitir
                END_EĞER
            END_EĞER
        END_DÖNGÜ

        // PIN Doğru İse devam et
        EĞER PIN_GIRIS = Hesabın_Dogru_PIN_Kodu İSE
            EKRANA_YAZ "Lütfen çekmek istediğiniz miktarı giriniz."
            CEKILECEK_MIKTAR = KULLANICIDAN_AL

            MEVCUT_BAKIYE = BANKA_SISTEMINDEN_AL(Hesap_No)
            ATM_KASA_DURUMU = ATM_KASASINDAN_AL

            EĞER CEKILECEK_MIKTAR > 0 VE (CEKILECEK_MIKTAR MOD Min_Cekim_Birimi) = 0 İSE
                EĞER CEKILECEK_MIKTAR <= MEVCUT_BAKIYE İSE
                    EĞER CEKILECEK_MIKTAR <= ATM_KASA_DURUMU İSE
                        // İşlem Onayı
                        YENI_BAKIYE = MEVCUT_BAKIYE - CEKILECEK_MIKTAR
                        BANKA_SISTEMINI_GUNCELLE(Hesap_No, YENI_BAKIYE)
                        ATM_KASASINI_GUNCELLE(CEKILECEK_MIKTAR)

                        EKRANA_YAZ "İşleminiz gerçekleştiriliyor..."
                        PARA_VER(CEKILECEK_MIKTAR)
                        FIS_YAZDIR("Çekilen Miktar: " + CEKILECEK_MIKTAR + ", Kalan Bakiye: " + YENI_BAKIYE)
                        EKRANA_YAZ "Lütfen paranızı ve fişinizi alınız."
                    
                    DEĞİLSE
                        EKRANA_YAZ "Üzgünüz, ATM'de istenen miktarda para bulunmamaktadır."
                    END_EĞER
                
                DEĞİLSE
                    EKRANA_YAZ "Yetersiz bakiye. Mevcut bakiyeniz: " + MEVCUT_BAKIYE
                END_EĞER
            
            DEĞİLSE
                EKRANA_YAZ "Geçersiz miktar. Lütfen sıfırdan büyük ve " + Min_Cekim_Birimi + " katı bir miktar giriniz."
            END_EĞER

            // İşlem sonrası kartı iade et
            KARTI_IADE_ET
            EKRANA_YAZ "Teşekkürler. Kartınızı almayı unutmayınız."
        END_EĞER // PIN doğru kontrolünün sonu

    DEĞİLSE
        EKRANA_YAZ "Geçersiz Kart. Lütfen bankanızla iletişime geçiniz."
        KARTI_IADE_ET
    END_EĞER

BITIR
