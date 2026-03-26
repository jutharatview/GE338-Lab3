Lab 3 พื้นที่ศึกษา จังหวัดชัยนาท
ภารกิจที่ 1 ออกแบบ Training Strategy
จำแนกที่ดินบ่งเป็น 5 Class ดังนี้ 1 = Water 2 = Agriculture 3 = Forest 4 = Urban 5 = Miscellaneous 
หา Training Sample โดยการใช้ Dynamic World เนื่องจากประหยัดเวลา
แบ่ง Train เป็น Random Split 80/20 โดย 80 เป็น Train 20 เป็น Test ที่มีการใช้แบ่งแบบสุ่มเนื่องจากทำได้ง่ายและใช้อย่างแพร่หลาย เหมาะกับงานที่ไม่ได้ควบคุมความสัมพันธ์เชิงพื้นที่แบบชัดเจน ในส่วนของ Spatial Cross validation การแบ่งแบบสุ่มอาจจะประเมินความแม่นยำได้สูงเกินไป เนื่องจาก Pixel ที่อยู่ใกล้เคียงกันมีความคล้ายคลึงกัน Spatial Cross validation จะมีความซับซ้อนในการนำไปใช้มากกว่า
Feature ที่เลือกใช้จะมี Spectral Bands: B2 (Blue) B3 (Green) B4 (Red) B8 (NIR) B11 (SWIR)
Index ที่ใช้จะมี NDVI ใช้ในการตรวจจับพืชพรรณและแยกแยะระหว่างพื้นที่ที่มีพืชพรรณและพื้นที่ที่ไม่มีพืชพรรณ
NDWI ใช้เพื่อเพิ่มประสิทธิภาพในการตรวจจับแหล่งน้ำและแยกแหล่งน้ำออกจากพื้นดิน
NDBI ใช้ในการระบุพื้นที่เมืองหรือพื้นที่ที่มีสิ่งปลูกสร้าง 
การใช้ทั้งสอง Feature ช่วยในการแยกประเภทการใช้ประโยชน์ที่ดินและนำไปสู่ประสิทธิภาพการจำแนกที่ดี
ภารกิจที่ 2 การเปรียบเทียบ Algorithm
Random Forestทดลองจำนวนต้นไม้
- 50 trees
- 100 trees
- 200 trees
โดยผลลัพธ์ที่ได้ค่า Accuracy RF50 : 0.761 RF100 : 0.771 RF200 : 0.770
ค่า Kappa RF50 : 0.420 RF100 : 0.441 RF200: 0.438
โดยจำนวนต้นไม้ที่เพิ่มขึ้นช่วยเพิ่มความแม่นยำเล็กน้อย แต่หลัง 100 trees ประสิทธิภาพเริ่มคงที่
การเปรียบเทียบ Metrics 
Random Forest Accuracy PA ≈ 0.77 UA ≈ 0.43
ForestPA ≈ 0.96 UA ≈ 0.80 F1 ≈ 0.87
Agriculture PA ≈ 0.53 UA ≈ 0.84 F1 ≈ 0.65
Urban PA ≈ 0.47 UA ≈ 0.58 F1 ≈ 0.52
Agriculture / Urban ค่าไม่ค่อยดีอาจมีการตกหล่นของ class เหล่านี้ 
Miscellaneous PA ≈ 0.35 UA ≈ 0.64 F1 ≈ 0.45
Miscellaneous ค่าต่ำมากโมเดลแทบไม่สามารถจำแนก class นี้ได้
Water ค่า = 0 ยังมีปัญหา อาจจะมี sample น้อย หรือโดนกลบด้วย NDWI threshold 
User’s Accuracy (UA) บาง class มีค่า 0.5แปลว่ามี false positive อยู่พอสมควร 
F1-score Forest มีค่าสูง สมดุลทั้ง precision และ recall) 
Agriculture / Urban มีค่าปานกลาง Miscellaneous มีค่าต่ำมาก แสดงว่า model ไม่สมดุลในทุก class
ภารกิจที่ 3 
Feature Importance จาก Random Forest:
Feature สำคัญ NDVI ใช้แยกพืชได้ดี สำคัญที่สุด NDWI ใช้แยกน้ำ NDBI ใช้แยกเมือง 
หากตัด Feature ที่สำคัญต่ำออก เช่น B2 (Blue) B3 (Green) จะมีผลน้อยต่อการจำแนก ค่า accuracy จะลดลงเล็กน้อย
ภารกิจที่ 4: ประเมินความไม่แน่นอน 
โดย Class ที่สับสนมากที่สุดคือ Agriculture กับ Forest และ Urban กับ Miscellaneous เนื่องจาก มี spectral signature ใกล้กัน และ pixel มีการผสมกัน
พื้นที่ที่โมเดลไม่แน่ใจ พื้นที่เกษตรที่มีน้ำขัง พื้นที่รอยต่อเมืองกับชนบท พื้นที่ดินเปล่า
การสื่อสารกับ Stakeholder
แผนที่นี้มีความแม่นยำในระดับปานกลาง (~77%) เหมาะสำหรับการวิเคราะห์ภาพรวม แต่ไม่ควรใช้ในระดับรายละเอียดสูงหรือการตัดสินใจเชิงพื้นที่ที่สำคัญโดยตรง
คำถามเพิ่มเติม
1. ถ้าเพิ่ม Training Data แล้ว Accuracy มีแนวโน้มเพิ่มขึ้น โดยเฉพาะ class ที่มีข้อมูลน้อย
2. Spatial Autocorrelation จะทำให้ Accuracy สูงเกินจริง เพราะ train/test อยู่ใกล้กัน
3. Class ที่แย่ที่สุด Miscellaneous และ Urban โดยแนวทางแก้คือ เพิ่ม training data และ feature (texture / time-series)
4. ถ้าทำพื้นที่อื่นจะต้องเปลี่ยน Training Data และหา Feature ที่เหมาะกับพื้นที่
โดยสิ่งที่ใช้ซ้ำได้ Workflow Code และ Model structure
