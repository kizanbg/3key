# Moja Handwire Tastatura - ZMK Firmware

3-tasterska handwire tastatura, Nice Nano v2, direct pin (bez matrice, bez dioda).

## Struktura fajlova

```
zmk-config/
├── build.yaml                          # Koji board/shield da buildujes
├── config/
│   ├── west.yml                        # ZMK verzija (main branch)
│   ├── moja_tastatura.conf             # Opcije (BLE ime, sleep, itd.)
│   └── boards/shields/moja_tastatura/
│       ├── moja_tastatura.overlay      # Pinovi i kscan konfiguracija
│       ├── moja_tastatura.keymap       # Tastaturne kombinacije (keymapa)
│       ├── moja_tastatura.zmk.yml      # Metapodaci shielda
│       ├── Kconfig.defconfig           # Ime tastature
│       └── Kconfig.shield              # Shield flag
└── .github/workflows/
    └── build.yml                       # GitHub Actions - automatski build
```

## Kako promeniti pinove

Otvori `config/boards/shields/moja_tastatura/moja_tastatura.overlay`

```dts
input-gpios
    = <&pro_micro 4 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>   /* Taster 1 */
    , <&pro_micro 5 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>   /* Taster 2 */
    , <&pro_micro 6 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>   /* Taster 3 */
    ;
```

Zameni broj (4, 5, 6) sa pro_micro brojem pina koji koristis.

### Pro Micro -> Nice Nano mapa pinova

| pro_micro broj | Nice Nano pin |
|:--------------:|:-------------:|
| 0              | D3  (P0.06)   |
| 1              | D2  (P0.08)   |
| 2              | D1  (P0.17)   |
| 3              | D0  (P0.20)   |
| 4              | D4  (P0.22)   |
| 5              | C6  (P0.24)   |
| 6              | D7  (P0.09)   |
| 7              | E6  (P0.10)   |
| 8              | B4  (P0.11)   |
| 9              | B5  (P1.04)   |

Puna referenca: https://nicekeyboards.com/docs/nice-nano/pinout-schematic

## Kako promeniti keymap

Otvori `config/boards/shields/moja_tastatura/moja_tastatura.keymap`

```dts
bindings = <
    &kp A    &kp B    &kp C
>;
```

Zameni `A`, `B`, `C` sa keycodeovima po zelji.
Lista svih keycodeova: https://zmk.dev/docs/codes

## Build (GitHub Actions)

1. Napravi GitHub repo i pushuj ovaj folder
2. GitHub Actions automatski builda firmware
3. Idi na Actions tab -> poslednji job -> Artifacts -> skini ZIP
4. Flashuj `moja_tastatura-nice_nano_v2-zmk.uf2` na Nice Nano (double-tap reset)

## Fizicko vezivanje

```
Taster 1: pin D4 ----[SW1]---- GND
Taster 2: pin C6 ----[SW2]---- GND
Taster 3: pin D7 ----[SW3]---- GND
```

Nema dioda, nema matrice - direktna veza pin -> taster -> GND.
