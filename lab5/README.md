# Architecture Decision Record (ADR)
## Online Food Ordering System
1. Requirements Analysis
Functional Requirements

ผู้ใช้สามารถสมัครสมาชิกและเข้าสู่ระบบได้

ผู้ใช้สามารถเลือกดูเมนูอาหารจากร้านต่างๆ และสั่งอาหารได้

ระบบสามารถติดตามสถานะการสั่งอาหารแบบ Real-time

ระบบรองรับการชำระเงินผ่าน QR Code และ Credit Card

ระบบแจ้งเตือนสถานะคำสั่งซื้อผ่าน Email หรือ LINE

ร้านค้าสามารถจัดการเมนู ราคา และสถานะร้านได้

ผู้ดูแลระบบสามารถจัดการผู้ใช้ ร้านค้า และคำสั่งซื้อได้

Non-Functional Requirements

Performance: ระบบต้องตอบสนองการเปลี่ยนสถานะคำสั่งซื้อภายใน 2 วินาที

Scalability: ระบบต้องรองรับผู้ใช้งานพร้อมกันอย่างน้อย 10,000 คน

Availability: ระบบต้องมี uptime อย่างน้อย 99.9%

Security: ข้อมูลการชำระเงินต้องถูกเข้ารหัสและเป็นไปตามมาตรฐานความปลอดภัย

Maintainability: ระบบต้องสามารถเพิ่มฟีเจอร์ใหม่ได้โดยกระทบระบบเดิมน้อยที่สุด

Constraints

Technology Constraint: ต้องพัฒนาโดยใช้ Web-based Technology

Budget Constraint: ใช้ Open-source tools เป็นหลักเพื่อลดค่าใช้จ่าย

Time Constraint: ต้องพัฒนา MVP ให้เสร็จภายใน 1 ภาคการศึกษา

Integration Constraint: ต้องเชื่อมต่อ Payment Gateway ภายนอก

Quality Attribute Scenarios
Scenario 1: Real-time Order Tracking

Quality Attribute: Performance

Source: ลูกค้า

Stimulus: ลูกค้าตรวจสอบสถานะคำสั่งซื้อ

Artifact: Order Tracking Service

Environment: ระบบทำงานปกติ มีผู้ใช้งานพร้อมกันจำนวนมาก

Response: ระบบแสดงสถานะล่าสุดของคำสั่งซื้อ

Response Measure: ภายใน 2 วินาที

Scenario 2: Traffic Spike During Lunch Time

Quality Attribute: Scalability

Source: ผู้ใช้งานจำนวนมาก

Stimulus: มีการสั่งอาหารพร้อมกันจำนวนมากในช่วงเวลาเร่งด่วน

Artifact: Backend Services

Environment: Load สูง

Response: ระบบสามารถ scale อัตโนมัติ

Response Measure: ไม่มี request ล้มเหลวเกิน 1%

2. Candidate Architectures
Candidate Architecture 1: Monolithic Architecture
Overview

ระบบทั้งหมดถูกรวมอยู่ในแอปพลิเคชันเดียว เหมาะสำหรับทีมขนาดเล็กและการพัฒนาเริ่มต้น

Components

User Management Module

Order Management Module

Payment Module

Notification Module

Admin Dashboard

Technology Stack

Frontend: React.js

Backend: Node.js (Express)

Database: MySQL

Others: REST API, JWT Authentication

Architectural Patterns

Monolithic Architecture

Layered Architecture

Diagram
[ Client ]
     |
[ Frontend ]
     |
[ Monolithic Backend ]
     |
[ Database ]

Pros & Cons

Pros:

✅ พัฒนาและ deploy ง่าย

✅ เหมาะสำหรับ MVP

✅ ค่าใช้จ่ายต่ำ

Cons:

❌ ขยายระบบยากเมื่อผู้ใช้เพิ่ม

❌ การแก้ไขบางส่วนอาจกระทบทั้งระบบ

❌ ไม่เหมาะกับระบบขนาดใหญ่ในระยะยาว

Candidate Architecture 2: Microservices Architecture
Overview

แยกระบบออกเป็นบริการย่อย แต่ละ service ทำงานอิสระ เหมาะสำหรับระบบที่ต้องการ scalability สูง

Components

User Service

Order Service

Payment Service

Notification Service

API Gateway

Technology Stack

Frontend: React.js

Backend: Node.js (NestJS)

Database: PostgreSQL (per service)

Others: Docker, Kubernetes, REST API, Message Queue (RabbitMQ)

Architectural Patterns

Microservices Architecture

API Gateway Pattern

Event-driven Architecture

Diagram
[ Client ]
     |
[ API Gateway ]
     |
--------------------------------
| User | Order | Payment | Noti |
--------------------------------
     |
[ Databases ]

Pros & Cons

Pros:

✅ Scalability สูง

✅ แต่ละ service พัฒนาแยกอิสระ

✅ เหมาะกับระบบขนาดใหญ่

Cons:

❌ ความซับซ้อนสูง

❌ ใช้ทรัพยากรมาก

❌ ต้องมี DevOps knowledge

3. Evaluation
Comparison Table
Criteria	Weight	Monolith (Score)	Monolith (Weighted)	Microservices (Score)	Microservices (Weighted)
Performance	20%	4	0.8	5	1.0
Scalability	25%	2	0.5	5	1.25
Maintainability	20%	3	0.6	5	1.0
Complexity	15%	5	0.75	2	0.3
Cost	20%	5	1.0	3	0.6
Total	100%		3.65		4.15
Selected Architecture

Decision: Microservices Architecture

Reasons:

รองรับการขยายระบบในอนาคตได้ดีกว่า

ตอบโจทย์ Real-time tracking และ traffic สูง

เหมาะกับระบบ Online Food Ordering ที่มีหลายฟังก์ชันแยกชัดเจน

