## **🛠 Technical Addendum: Infrastructure & Integration**

This section outlines the "plumbing" of CareSplit, focusing on how we use **Interswitch** for payments and **NIMC** for identity to ensure security and trust.

## **1\. Identity & Verification (The Trust Layer)**

By 2026, Nigeria’s Digital Public Infrastructure (DPI) allows for real-time identity validation.

* **KYC Tiering (Know Your Customer):**  
  * **NIN/BVN Lookup:** Using APIs (like Smile ID or directly via NIMC), we fetch the patient’s legal name, date of birth, and linked phone number.  
  * **Biometric Verification:** During onboarding, the app performs a liveness check (facial recognition) to match the NIN database photo, preventing identity theft.  
* **Risk Anchoring:** Linking the **BVN (Bank Verification Number)** allows the system to verify the stability of the user's financial identity without requiring a full credit history.

## **2\. Payment Rails (Interswitch Integration)**

CareSplit acts as an orchestration layer on top of **Interswitch’s** robust payment infrastructure.

* **Card Tokenization:**  
  * When the patient pays the first 25% deposit, Interswitch "tokenizes" the card details.  
  * CareSplit never stores actual card numbers; we store a secure "token" that allows us to trigger future monthly debits automatically.  
* **Recurring Debit (Tokenized Billing):**  
  * The system automatically triggers a debit of **₦30,000** (using the "Ada" persona example) on a set date each month.  
  * **Failed Transaction Logic:** If a debit fails, the system automatically retries and sends a notification to both the patient and the clinic dashboard.

## **3\. The "Open Clinic" API Strategy**

To become the **"Stripe for Healthcare Installments,"** CareSplit uses a flexible API structure.

* **Payment Link Generation:** Clinics can generate a unique URL via their dashboard. This link carries the **Care Plan ID**, total cost, and the specific installment breakdown.  
* **Instant Settlement:** Once Interswitch confirms a successful debit, our backend splits the funds. The processing fee (1.5%–2.5%) is routed to CareSplit, and the remainder is settled into the clinic's bank account.

---

## 

## 

## **📊 Data Flow Summary**

| Step | Action | Tech Component |
| :---- | :---- | :---- |
| **1\. Identity** | The patient enters NIN/BVN for verification. | NIMC / Smile ID API |
| **2\. Split** | The system calculates monthly installments. | CareSplit Logic Engine |
| **3\. Authorize** | The patient pays a deposit and agrees to auto-debit. | Interswitch Pay-with-Token |
| **4\. Payout** | Funds are settled into the clinic's account. | Interswitch Disbursement API |

---

## **🔒 Security & Compliance**

* **NDPR Compliance:** All data is encrypted and handled according to the Nigeria Data Protection Regulation.  
* **Data Encryption:** Sensitive patient care plans and financial records are encrypted at rest and in transit.

