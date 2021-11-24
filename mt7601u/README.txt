Para hacer andar el dispositivo mt7601u en Linux:

1. Bajar de kernel.org el código fuente del núcleo linux correspondiente al que se esté usando. Por ejemplo si se usa 4.4.0-104-generic, bajar la versión 4.4 .

2. Desempaquetar el archivo descargado y ubicar la carpeta o directorio drivers/net/wireless/mediatek/mt7601u

3. Editar el archivo phy.c. Ubicar la function mt7601u_init_cal y comentar con //  la llamada a mt7601u_mcu_calibrate(dev, MCU_CAL_RXIQ, 0); de esta manera:


  // ret = mt7601u_mcu_calibrate(dev, MCU_CAL_RXIQ, 0);
  // if (ret)
  //    return ret;
  // ret = mt7601u_mcu_calibrate(dev, MCU_CAL_DPD, dev->dpd_temp);
  // if (ret)
  //    return ret;


Encontrar la function mt7601u_phy_recalibrate_after_assoc y comentar con // la llamada mt7601u_mcu_calibrate(dev, MCU_CAL_DPD, dev->curr_temp); de esta manera:


  void mt7601u phy_recalibrate_after_assoc(struct mt7601u_dev *dev)
  {
  //  mt7c01u_mu calibrate(dev, WCU_CAL_DPO, dev->curr_temp);

      mt7601u_rxde_cal(dev);
  }

  
4. Construir el modulo: make -C /lib/modules/$(unane -r)/build M=$(pwd) modules

5. Remover el dispositivo 

6. Y quitar el módulo viejo del núcleo: sudo rmmod mt7601u

7. Cargar el módulo nuevo: sudo insmod ./mt7601u.ko

8. Insertar el dispositivo

9. Chequear que no haya errores con dmesg y que aparezca la interfaz , chequear que la conexión sea estable.

10. Para hacer que los cambios sean persistentes ante una próxima actualización del núcleo Linux: haga una copia de seguridad del módulo original y reemplácelo con el compilado

Para saber dónde está el módulo original, ejecute modinfo mt7601u (la ruta donde se encuentra el archivo es: /1ib/modules/_KERNEL_VERSION_/kernel/drivers/net/wireless/mediatek/mt7601u/mt7601u.ko ).

