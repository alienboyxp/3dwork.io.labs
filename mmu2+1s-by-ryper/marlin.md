---
description: >-
  A continuación te indicamos los cambios a realizar si decidiste usar Marlin
  para tu MMU2S+1S
---

# Marlin

## Ajustar el numero de Extrusores y habilitar MMU2

En configuration.h realizaremos los siguientes cambios.

Añadiremos un serial más para la comunicación con nuestro MMU2+1S, en nuestro ejemplo para una placa SKR habilitaremos el puerto 3:

```cpp
/**
 * Select a tertiary serial port on the board to use for communication with the host.
 * :[-1, 0, 1, 2, 3, 4, 5, 6, 7]
 */
#define SERIAL_PORT_3 3
```

Ajustar el numero de Extrusores al numero de tools/filementos de nuestro MMU2, en nuestro caso usaremos 7:

```cpp
#define EXTRUDERS 7
```

También habilitaremos el MMU2:

```cpp
#define PRUSA_MMU2
```

## Añadir la configuración del serial a usar para MMU2

Iremos a configuration\_adv.h y configuraremos el serial a usar para que Marlin se comunique con nuestro MMU2+1S:

```cpp
#if ENABLED(PRUSA_MMU2)

  // Serial port used for communication with MMU2.
  // For AVR enable the UART port used for the MMU. (e.g., mmuSerial)
  // For 32-bit boards check your HAL for available serial ports. (e.g., Serial2)
  #define MMU2_SERIAL_PORT 3
  #define MMU2_SERIAL Serial3

  // Use hardware reset for MMU if a pin is defined for it
  #define MMU2_RST_PIN P0_26

  // Enable if the MMU2 has 12V stepper motors (MMU2 Firmware 1.0.2 and up)
  //#define MMU2_MODE_12V

  // G-code to execute when MMU2 F.I.N.D.A. probe detects filament runout
  #define MMU2_FILAMENT_RUNOUT_SCRIPT "M600"

  // Add an LCD menu for MMU2
  #define MMU2_MENUS
  #if ENABLED(MMU2_MENUS)
    // Settings for filament load / unload from the LCD menu.
    // This is for Průša MK3-style extruders. Customize for your hardware.
    #define MMU2_FILAMENTCHANGE_EJECT_FEED 80.0
    #define MMU2_LOAD_TO_NOZZLE_SEQUENCE \
      {  7.2, 1145 }, \
      { 14.4,  871 }, \
      { 36.0, 1393 }, \
      { 14.4,  871 }, \
      { 50.0,  198 }

    #define MMU2_RAMMING_SEQUENCE \
      {   1.0, 1000 }, \
      {   1.0, 1500 }, \
      {   2.0, 2000 }, \
      {   1.5, 3000 }, \
      {   2.5, 4000 }, \
      { -15.0, 5000 }, \
      { -14.0, 1200 }, \
      {  -6.0,  600 }, \
      {  10.0,  700 }, \
      { -10.0,  400 }, \
      { -50.0, 2000 }
  #endif
```

## Ajustar numero de Tools/Filamentos si usamos &gt;5

Normalmente las unidades MMU2 vienen con 5 tools/filamentos, en el caso que uses MMU2S+1S puedes optar por las versiones de 7 o 10 tools/filamentos. En este caso deberás realizar los siguientes cambios en tu Marlin \(este documento esta basado en Marlin 2.0.7.2\) para poder manejar y usarlos:

### Ajuste de las opciones de menu de MMU2

editaremos el fichero /Marlin/src/feature/mmu2/mmu2.cpp y comenzaremos por la definicion de comandos originalmente encontramos así:

```cpp
void menu_mmu2_load_filament() {
  START_MENU();
  BACK_ITEM(MSG_MMU2_MENU);
  ACTION_ITEM(MSG_MMU2_ALL, action_mmu2_load_all);
  LOOP_L_N(i, 5) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ _mmu2_load_filament(MenuItemBase::itemIndex); });
  END_MENU();
}

void menu_mmu2_load_to_nozzle() {
  START_MENU();
  BACK_ITEM(MSG_MMU2_MENU);
  LOOP_L_N(i, 5) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ action_mmu2_load_filament_to_nozzle(MenuItemBase::itemIndex); });
  END_MENU();
  
// UNAS LINEAS MAS ABAJO

void menu_mmu2_eject_filament() {
  START_MENU();
  BACK_ITEM(MSG_MMU2_MENU);
  LOOP_L_N(i, 5) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ _mmu2_eject_filament(MenuItemBase::itemIndex); });
  END_MENU();
}

// UNAS LINEAS MAS ABAJO

void menu_mmu2_choose_filament() {
  START_MENU();
  #if LCD_HEIGHT > 2
    STATIC_ITEM(MSG_MMU2_CHOOSE_FILAMENT_HEADER, SS_DEFAULT|SS_INVERT);
  #endif
  LOOP_L_N(i, 5) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ action_mmu2_choose(MenuItemBase::itemIndex); });
  END_MENU();
}
```

De nuevo usaremos el caso de 7 donde cambiaremos los LOOP para que en lugar de un valor fijo de 5 usen la variable EXTRUDERS que se definio en configuration.h y que se adaptará dinamicamente al numero de tools/filamentos que pongamos:

```cpp
void menu_mmu2_load_filament() {
  START_MENU();
  BACK_ITEM(MSG_MMU2_MENU);
  ACTION_ITEM(MSG_MMU2_ALL, action_mmu2_load_all);
  LOOP_L_N(i, EXTRUDERS) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ _mmu2_load_filament(MenuItemBase::itemIndex); });
  END_MENU();
}

void menu_mmu2_load_to_nozzle() {
  START_MENU();
  BACK_ITEM(MSG_MMU2_MENU);
  LOOP_L_N(i, EXTRUDERS) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ action_mmu2_load_filament_to_nozzle(MenuItemBase::itemIndex); });
  END_MENU();
  
// UNAS LINEAS MAS ABAJO

void menu_mmu2_eject_filament() {
  START_MENU();
  BACK_ITEM(MSG_MMU2_MENU);
  LOOP_L_N(i, EXTRUDERS) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ _mmu2_eject_filament(MenuItemBase::itemIndex); });
  END_MENU();
}

// UNAS LINEAS MAS ABAJO

void menu_mmu2_choose_filament() {
  START_MENU();
  #if LCD_HEIGHT > 2
    STATIC_ITEM(MSG_MMU2_CHOOSE_FILAMENT_HEADER, SS_DEFAULT|SS_INVERT);
  #endif
  LOOP_L_N(i, EXTRUDERS) ACTION_ITEM_N(i, MSG_MMU2_FILAMENT_N, []{ action_mmu2_choose(MenuItemBase::itemIndex); });
  END_MENU();
}
```

### Ajuste de número de tools para MMU2

Para ello editaremos el fichero /Marlin/src/feature/mmu2/mmu2.cpp y comenzaremos por la definicion de comandos originalmente encontramos así:

```cpp
#define MMU_CMD_NONE 0
#define MMU_CMD_T0   0x10
#define MMU_CMD_T1   0x11
#define MMU_CMD_T2   0x12
#define MMU_CMD_T3   0x13
#define MMU_CMD_T4   0x14
#define MMU_CMD_L0   0x20
#define MMU_CMD_L1   0x21
#define MMU_CMD_L2   0x22
#define MMU_CMD_L3   0x23
#define MMU_CMD_L4   0x24
#define MMU_CMD_C0   0x30
#define MMU_CMD_U0   0x40
#define MMU_CMD_E0   0x50
#define MMU_CMD_E1   0x51
#define MMU_CMD_E2   0x52
#define MMU_CMD_E3   0x53
#define MMU_CMD_E4   0x54
#define MMU_CMD_R0   0x60
#define MMU_CMD_F0   0x70
#define MMU_CMD_F1   0x71
#define MMU_CMD_F2   0x72
#define MMU_CMD_F3   0x73
#define MMU_CMD_F4   0x74
```

Y deberemos ajustarlo para dejarlo de esta forma para un MMU2S+1S de 7 tools/filamentos:

```cpp
#define MMU_CMD_NONE 0
#define MMU_CMD_T0   0x10
#define MMU_CMD_T1   0x11
#define MMU_CMD_T2   0x12
#define MMU_CMD_T3   0x13
#define MMU_CMD_T4   0x14
#define MMU_CMD_T5   0x15
#define MMU_CMD_T6   0x16
#define MMU_CMD_L0   0x20
#define MMU_CMD_L1   0x21
#define MMU_CMD_L2   0x22
#define MMU_CMD_L3   0x23
#define MMU_CMD_L4   0x24
#define MMU_CMD_L5   0x25
#define MMU_CMD_L6   0x26
#define MMU_CMD_C0   0x30
#define MMU_CMD_U0   0x40
#define MMU_CMD_E0   0x50
#define MMU_CMD_E1   0x51
#define MMU_CMD_E2   0x52
#define MMU_CMD_E3   0x53
#define MMU_CMD_E4   0x54
#define MMU_CMD_E5   0x55
#define MMU_CMD_E6   0x56
#define MMU_CMD_R0   0x60
#define MMU_CMD_F0   0x70
#define MMU_CMD_F1   0x71
#define MMU_CMD_F2   0x72
#define MMU_CMD_F3   0x73
#define MMU_CMD_F4   0x74
#define MMU_CMD_F5   0x75
#define MMU_CMD_F6   0x76
```

A continuación actualizaremos los bucles para que se adapten a nuestro numero de tools/filamentos que originalmente se encuntran asi:

```cpp
case 1:
      if (cmd) {
        if (WITHIN(cmd, MMU_CMD_T0, MMU_CMD_T4)) {
          // tool change
          int filament = cmd - MMU_CMD_T0;
          DEBUG_ECHOLNPAIR("MMU <= T", filament);
          tx_printf_P(PSTR("T%d\n"), filament);
          TERN_(MMU_EXTRUDER_SENSOR, mmu_idl_sens = 1); // enable idler sensor, if any
          state = 3; // wait for response
        }
        else if (WITHIN(cmd, MMU_CMD_L0, MMU_CMD_L4)) {
          // load
          int filament = cmd - MMU_CMD_L0;
          DEBUG_ECHOLNPAIR("MMU <= L", filament);
          tx_printf_P(PSTR("L%d\n"), filament);
          state = 3; // wait for response
        }
        else if (cmd == MMU_CMD_C0) {
          // continue loading
          DEBUG_ECHOLNPGM("MMU <= 'C0'");
          tx_str_P(PSTR("C0\n"));
          state = 3; // wait for response
        }
        else if (cmd == MMU_CMD_U0) {
          // unload current
          DEBUG_ECHOLNPGM("MMU <= 'U0'");

          tx_str_P(PSTR("U0\n"));
          state = 3; // wait for response
        }
        else if (WITHIN(cmd, MMU_CMD_E0, MMU_CMD_E4)) {
          // eject filament
          int filament = cmd - MMU_CMD_E0;
          DEBUG_ECHOLNPAIR("MMU <= E", filament);
          tx_printf_P(PSTR("E%d\n"), filament);
          state = 3; // wait for response
        }
        else if (cmd == MMU_CMD_R0) {
          // recover after eject
          DEBUG_ECHOLNPGM("MMU <= 'R0'");
          tx_str_P(PSTR("R0\n"));
          state = 3; // wait for response
        }
        else if (WITHIN(cmd, MMU_CMD_F0, MMU_CMD_F4)) {
          // filament type
          int filament = cmd - MMU_CMD_F0;
          DEBUG_ECHOPAIR("MMU <= F", filament, " ");
          DEBUG_ECHO_F(cmd_arg, DEC);
          DEBUG_EOL();
          tx_printf_P(PSTR("F%d %d\n"), filament, cmd_arg);
          state = 3; // wait for response
        }
```

De nuevo usaremos el caso de 7 donde cambiamos la definicion de T4, L4, E4 y F4 por el que se adapte a nuestra necesidad... en el caso del ejemplo al tener 7 tools/filamentos cambiaremos por T6, L6, E6 y F6 quedando esa parte del código de la siguiente forma:

```cpp
case 1:
      if (cmd) {
        if (WITHIN(cmd, MMU_CMD_T0, MMU_CMD_T6)) {
          // tool change
          int filament = cmd - MMU_CMD_T0;
          DEBUG_ECHOLNPAIR("MMU <= T", filament);
          tx_printf_P(PSTR("T%d\n"), filament);
          TERN_(MMU_EXTRUDER_SENSOR, mmu_idl_sens = 1); // enable idler sensor, if any
          state = 3; // wait for response
        }
        else if (WITHIN(cmd, MMU_CMD_L0, MMU_CMD_L6)) {
          // load
          int filament = cmd - MMU_CMD_L0;
          DEBUG_ECHOLNPAIR("MMU <= L", filament);
          tx_printf_P(PSTR("L%d\n"), filament);
          state = 3; // wait for response
        }
        else if (cmd == MMU_CMD_C0) {
          // continue loading
          DEBUG_ECHOLNPGM("MMU <= 'C0'");
          tx_str_P(PSTR("C0\n"));
          state = 3; // wait for response
        }
        else if (cmd == MMU_CMD_U0) {
          // unload current
          DEBUG_ECHOLNPGM("MMU <= 'U0'");

          tx_str_P(PSTR("U0\n"));
          state = 3; // wait for response
        }
        else if (WITHIN(cmd, MMU_CMD_E0, MMU_CMD_E6)) {
          // eject filament
          int filament = cmd - MMU_CMD_E0;
          DEBUG_ECHOLNPAIR("MMU <= E", filament);
          tx_printf_P(PSTR("E%d\n"), filament);
          state = 3; // wait for response
        }
        else if (cmd == MMU_CMD_R0) {
          // recover after eject
          DEBUG_ECHOLNPGM("MMU <= 'R0'");
          tx_str_P(PSTR("R0\n"));
          state = 3; // wait for response
        }
        else if (WITHIN(cmd, MMU_CMD_F0, MMU_CMD_F6)) {
          // filament type
          int filament = cmd - MMU_CMD_F0;
          DEBUG_ECHOPAIR("MMU <= F", filament, " ");
          DEBUG_ECHO_F(cmd_arg, DEC);
          DEBUG_EOL();
          tx_printf_P(PSTR("F%d %d\n"), filament, cmd_arg);
          state = 3; // wait for response
        }
```

> Agradecimientos a...  
> **MAXI** de la [**comunidad Ryper 3D**](https://t.me/RYPER3D) por la recopilación de cambios y tests

