
BUSINESS PROBLEM
Globex Retail, a company specializing in e-commerce and brick-and-mortar sales, employed services of a Data Engineer / Data Analyst to analyze a dataset to understand customer purchase behavior. 
Globex Retail wants to: retain customers, optimize inventory to match demand, grow revenue with targeted marketing, and make faster, data-driven decisions.

ETL PROCESS (Extract → Transform → Load)

To ensure reliable insights,  a structured ETL workflow was designed:

Extract

Raw data originated in a CSV containing transactional details (Customer_ID, Order_ID, Order_Date, Product_Category, Product_Sub_Category, Quantity, Price, Discount, Customer_Location).
Environment variables for PostgreSQL connection (DB_USER, DB_PASSWORD, DB_HOST, DB_PORT, DB_NAME) were securely managed through a .env file.

Transform

Data was cleaned and transformed by:
Choosing appropriate data types
Customer segmentation was created with a new field high_value_customers (True/False), defined as customers whose lifetime revenue is above the 80th percentile.
Data Validation was done to ensure the transformation aligned with business needs.

Star schema modeling:

Dimension tables: customer (customer location, high-value flag), order (date, month, weekday), product (category, sub-category with surrogate Product_ID).
Fact table: transactional measures (Quantity, Price, Discount, Revenue) keyed by Customer_ID, Order_ID, and Product_ID.

Load

Using SQLAlchemy/psycopg2, all tables were loaded into a PostgreSQL database:
Tables were written via .to_sql() with if_exists="replace" to ensure idempotent loading during testing.
This ETL not only standardized data but also prepared a foundation for analytics and dashboarding in BI tools.

KEY INSIGHTS (Pandas EDA)

Revenue leaders: Electronics dominates overall revenue; top subcategories include Smart Watches, Smartphones, Tablets, Gaming Consoles, Headphones, and Laptops.

Discount dynamics: ~75% of total revenue occurs at low discounts (<5%); the top-grossing discount band across categories is typically 0–5%.

High-value customers: They show the highest AOV and receive slightly lower average discounts than the overall base. The highest concentration of high-value customers clusters in states such as CO, AL, and IL when measured by share.

Time trends: Revenue peaks mid-year (e.g., July 2023) with strong months in early 2024 (Jan–Feb). AOV fluctuates by month, suggesting timing matters for promotions and replenishment.


RECOMMENDATIONS

Merchandising & inventory: Prioritize top-earning electronics subcategories for depth and availability.

Pricing & promotions: Keep discounts modest in core categories; test targeted 5–10% promos for slower lines rather than blanket markdowns.

Customer strategy: Formalize a “High-Value” tier (top 20%) with tailored perks that don’t rely on heavy discounts; use lifetime value to drive retention.

Geo targeting: Concentrate campaigns in states with the highest density and count of high-value customers; seed adjacent states with look-alike offers.

Planning cadence: Align inventory and marketing pushes around months with historically higher AOV/Revenue; pre-position stock and staffing for those spikes.

Measurement: Track discount bands vs revenue mix by category monthly; keep a watchlist of subcategories that respond best to small, targeted discounts.
