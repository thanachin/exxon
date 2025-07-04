# exxon
Exxon_SourceCode
#How to clean Data
อันดับแรกพวกเรา clean file inbound , outbound , Inventory
โดยจัดการกับ duplicate data โดยการยุบให้เหลือ 1
จัดการ missing value ด้วยการลบทิ้งเนื่องจากมีน้อย
จัดการ outlier ด้วยการแยกไฟล์ csv ออกเป็น 2 ไฟล์
ไฟล์ที่ถูก clean outlier จนหมด กับ ไฟล์ ที่มีแต่ outlier

#Add Feature
1.inbound join ร่วมกับ materail
2.เพิ่ม เวลา เรื่องหมดอายุ
3.merge inbound outbound 
4.เพิ้ม column เดือนที่ควร outbound 
5.แปลงสกุลเงินให้เป็น dolla
6.หา row ไหนบ้างที่ ค่าcontainer รับไม่ไหว
แยก date เดือน/ปี groupby netquality ตามเดือน ตาม week แปลงเป็น กิโลตัน
วันหยุดหรือไม่ อิงตามไฟล์ wh_calender ตามประเทศนั้นๆ

#อธิบายว่าไฟล์แต่ละไฟลทำงานยังไง
1.outbound_prepare.ipynb ทำการ clean data outbound.csv และคืนไฟล์ outbound_clean

2.Inbound_Cleaned(All_dup).ipynp ทำการ clean data inbound.csv และคืนไฟล์ 'Inbound_Cleaned(All dup).csv'

3.inventory_prepare.ipynb ทำการ clean data inventory_prepare.csv และคืนไฟล์ 2 ไฟล์
inventory_perfect.csv อยู่ในช่วง IQR และ inventory_outler.csv ที่เป็น outlier

4.Feature_Engineering_andMore.ipynb อ่านไฟล์ 'Inbound_Cleaned(All dup) (4).csv' และ 'MaterialMaster.csv' ทำการmerge กัน
และอ่านไฟล์ outbound_clean.csv , 'OperationCost.csv' หาว่า NET_QUANTITY_MT เยอะกว่า 24.75ไหม
ทำการ คำนวน tranfer cost และคืนค่าไฟล์ outbound_group_withcost.csv ออกมา
ทำการ groupby เวลา inbound กับ NET_QUANTITY_MT และ ทำให้เป็นหน่วยกิโล
คืนไฟล์ inbound_group.csv และหาวันหยุด คืนไฟล์ 'outbound_mat_holiday_byweek.csv'

##การทำ ML 
1.Predict_STOCK_SELL_VALUE.ipynb ทำการ อ่านไฟล์ inventory_outlier.csv
สร้าง feature dolla , ทำ One hot encoding ,แบ่งข้อมูล 80 / 20
ใช้ model DecisionTreeRegressor , RandomForestRegressor , GradientBoostingRegressor
เพื่อเปรียบเทียบอันไหนดีที่สุด โดยภาพรวมคือ Random Forest Regressor

2.Forecasting_Hoilday_Impact_.ipynb ทำการอ่านไฟล์ outbound_mat_holiday_byweek.csv
ทำการวิเคราะห์ feature NET_QUANTITY_MT(ตัวแปรต้น) และ IS_HOLIDAY(ตัวแปรตาม)
ใช้ model SGDClassifier และ RandomForestClassifier ซึงผลที่ได้ไม่เป็นไปตามการคาดหวังเนื่องจาก
ข้อมูลมีความ imbalance มากจึงต้องสร้าง featureเพิ่ม เพิ่ม 'OUTBOUND_YEAR', 'OUTBOUND_WEEK', 'TIME_WEEK',  'PLANT_NAME_SINGAPORE-WAREHOUSE', 'MODE_OF_TRANSPORT_Truck'
จนผลที่ได้พบว่า RandomForestClassifier ได้ผลเป็นไปตามที่คาดหวัง

3.Time Seires and plot.ipynb ทำการ อ่านไฟล์ Inbound_with_SHELF_LIFE_IN_MONTH (1).csv
เป็นการ plot graph เพื่อหา insight

4.Inbound & Outbound Predict ทำการ อ่านไฟล์inbound_group (2).csv และ outbound_group (2).csv
นำ 2 ไฟล์ merge กัน 
หาว่า feature time และ feature NET_QUANTITY_KT_x , 'NET_QUANTITY_KT_y' มีผลต่อกันมากแค่ไหน
ผลที่ได้ มีผลต่อกันน้อยมาก





