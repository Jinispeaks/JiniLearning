What Are Slowly Changing Dimensions (SCD)?
SCDs are a technique used in data warehouses to manage historical data and changes in dimensions (like customer details) over time. There are different ways to handle these changes, called Types 1, 2, and 3.

➡️SCD Type 1 – Overwrite the Old Data
☑️Simply updates the existing record with new information
☑️No historical tracking

🌟Example🌟: Let’s say a customer changes their address. You don’t care about the old address, so you simply overwrite it with the new one.

➡️ SCD Type 2 – Track Historical Changes
☑️Keeps full history by adding a new row for each change
☑️Uses start and end dates to track validity

🌟Example🌟:When a customer changes their address, you keep the old address and mark it as inactive, then add a new row with the new address.

➡️ SCD Type 3 – Track Limited Historical Changes
☑️Keeps limited history by adding a new column for the changed attribute
☑️Typically used when you only need to track the previous value

🌟Example🌟:You want to keep track of both the old address and the new address but only for the most recent change.
