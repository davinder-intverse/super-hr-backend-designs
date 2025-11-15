---
config:
  theme: redux-dark
---
flowchart TB
    n2(["Start"]) --> n3["<b>REQUEST BODY</b><br>firstName<br>lastName<br>email<br>password<br>isCandidate<br>termsAndConditionsAccepted<br>termsAndConditionsVersion"]
    n3 --> n4["isCandidate == false?"]
    n4 -- true --> n5["Is email a work email?"]
    n5 -- false --> n7["throw error → Free email domain not allowed"]
    n5 -- true --> n6["Validate T&C version exists"]
    n6 -- no --> n8["throw error → Terms & Conditions version not found"]
    n6 -- yes --> n9["Does user already exist?"]
    n9 -- no --> n11["Hash password"]
    n11 --> n12["Create new account"]
    n12 --> n13["Record T&C acceptance"]
    n13 --> n14(["End →Save the verification token in the database. and token in email Queue"])
    n9 -- yes --> n15["Is the existing user a guest?"]
    n15 -- yes --> n17["Does the guest user already have a password?"]
    n17 -- no --> n20["Hash password"]
    n20 --> n21["Upgrade guest → Full account"]
    n21 --> n22["Record T&C acceptance"]
    n22 --> n23["Migrate previous guest application data"]
    n23 --> n14
    n17 -- yes --> n16["Is the existing user verified?"]
    n15 -- no --> n16
    n16 -- yes --> n18["throw error → User already exists"]
    n16 -- no --> n19["throw error → Account exists but email not verified"]
    n4 -- no --> n6
    n3@{ shape: rect}
    n4@{ shape: diam}
    n5@{ shape: diam}
    n7@{ shape: rect}
    n6@{ shape: diam}
    n8@{ shape: rect}
    n9@{ shape: diam}
    n15@{ shape: diam}
    n17@{ shape: diam}
    n16@{ shape: diam}
    n18@{ shape: rect}
    n19@{ shape: rect}
     n2:::startNode
     n7:::errorNode
     n8:::errorNode
     n14:::endNode
     n18:::errorNode
     n19:::errorNode
    classDef errorNode stroke:#FF0033, stroke-width:2px, fill:#FFE5E9, color:#8E001A
    classDef startNode stroke:#00B36B, stroke-width:2px, fill:#D7FFE9, color:#035E3F
    classDef endNode stroke:#007BFF, stroke-width:2px, fill:#E0ECFF, color:#003A80
