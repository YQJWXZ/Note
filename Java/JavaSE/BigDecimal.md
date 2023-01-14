# BigDecimal
用于小数的精确运算

构造方法： BigDecimal(String val)

四则运算：
* add(BigDecimal b)
* subtract(BigDecimal b)
* multiply(BigDecimal b)
* divide(BigDecimal b)

除法注意事项：
public BigDecimal divide(另一个BigDecimal对象，精确几位，摄入模式)
        RoundingMode.UP  进一法
        RoundingMode.FLOOR  去尾法
        RoundingMode.HALF_UP  四舍五入