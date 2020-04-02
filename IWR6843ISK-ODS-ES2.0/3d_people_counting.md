## UART接口

### 传输顺序
1. Pcount3DDemo_output_message_header
> 和之前总头保持一致
```
typedef struct Pcount3DDemo_output_message_header_t
{
    /*! @brief   Output buffer magic word (sync word). It is initialized to  {0x0102,0x0304,0x0506,0x0708} */
    uint16_t    magicWord[4];

    /*! brief   Version: : MajorNum * 2^24 + MinorNum * 2^16 + BugfixNum * 2^8 + BuildNum   */
    uint32_t     version;

    /*! @brief   Total packet length including header in Bytes */
    uint32_t    totalPacketLen;

    /*! @brief   platform type */
    uint32_t    platform;

    /*! @brief   Frame number */
    uint32_t    frameNumber;

    /*! @brief   For Advanced Frame config, this is the sub-frame number in the range
     * 0 to (number of subframes - 1). For frame config (not advanced), this is always
     * set to 0. */
    uint32_t    subFrameNumber;

    /*! @brief Detection Layer timing */
    uint32_t    chirpProcessingMargin;
    uint32_t    frameProcessingTimeInUsec;

    /*! @brief Localization Layer Timing */
    uint32_t    trackingProcessingTimeInUsec;
    uint32_t    uartSendingTimeInUsec;


    /*! @brief   Number of TLVs */
    uint16_t    numTLVs;

    /*! @brief   check sum of the header */
    uint16_t    checkSum;

} Pcount3DDemo_output_message_header;
```

2. Pcount3DDemo_output_message_UARTpointCloud *objOut;
>  先是header和pointUnit,header一致，pointUnit是单位，方便把点的float转int，传输出来在乘以pointUnit还原
```
typedef struct MmwDemo_output_message_UARTpointCloud_t
{
    Pcount3DDemo_output_message_tl       header;
    Pcount3DDemo_output_message_point_unit pointUint;
    Pcount3DDemo_output_message_UARTpoint    point[MAX_RESOLVED_OBJECTS_PER_FRAME];
} Pcount3DDemo_output_message_UARTpointCloud;

typedef struct Pcount3DDemo_output_message_point_uint_t
{
    /*! @brief elevation  reporting unit, in radians */
    float       elevationUnit;
    /*! @brief azimuth  reporting unit, in radians */
    float       azimuthUnit;
    /*! @brief Doppler  reporting unit, in m/s */
    float       dopplerUnit;
    /*! @brief range reporting unit, in m */
    float       rangeUnit;
    /*! @brief SNR  reporting unit, linear */
    float       snrUint;

} Pcount3DDemo_output_message_point_unit;


typedef struct Pcount3DDemo_output_message_UARTpoint_t
{
    /*! @brief Detected point elevation, in number of azimuthUnit */
    int8_t      elevation;
    /*! @brief Detected point azimuth, in number of azimuthUnit */
    int8_t      azimuth;
    /*! @brief Detected point doppler, in number of dopplerUnit */
    int16_t      doppler;
    /*! @brief Detected point range, in number of rangeUnit */
    uint16_t        range;
    /*! @brief Range detection SNR, in number of snrUnit */
    uint16_t       snr;

} Pcount3DDemo_output_message_UARTpoint;
```

3. TargetList header&data
> 查看文档发现和之前相同
```
Pcount3DDemo_output_message_tl
gMmwMssMCB.trackerOutput.tList = (trackerProcObjType*)gObjDetObj->dpuTrackerObj)->targetDescrHandle->tList

```

4. TargetIndex header&data
> 查看文档发现和之前相同
```
Pcount3DDemo_output_message_tl
gMmwMssMCB.trackerOutput.tIndex = ((trackerProcObjType*)gObjDetObj->dpuTrackerObj)->targetDescrHandle->tIndex
```