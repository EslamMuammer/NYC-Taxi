# Total income from trips.
Total Revenue = SUM(Fact_Trips[total_amount])

# Average price of the trip without extras
AVG Fare Amount = AVERAGE(Fact_Trips[fare_amount])

# Average tip per trip
AVG Tip Amount = AVERAGE(Fact_Trips[tip_amount])

# Tip percentage of fare
Tip % = SUM(Fact_Trips[tip_amount]) / SUM(Fact_Trips[fare_amount])

# Additional income from government and regulatory fees
Total Surcharges = SUM(Fact_Trips[extra_charges]) + SUM(Fact_Trips[mta_tax]) + SUM(Fact_Trips[improvement_surcharge]) + SUM('Fact_Trips'[congestion_surcharge])


# Year-on-year revenue growth
Revenue Growth % = 
var PY Revenue = 
	CALCULATE(
    [Total Revenue], 
    SAMEPERIODLASTYEAR('Dim_Date'[Date])
	)
RETURN
	DIVIDE(
    [Total Revenue] - [PY Revenue], 
    [PY Revenue], 
    0
	)

# The cost of one trip
Avg Revenue per Trip = SUM(Fact_Trips[total_amount]) / COUNT(Fact_Trips[Vendor])

# Operating volume
Total Trips = COUNTROWS(Fact_Trips)

# Average Trip length
AVG Trip Distance (miles) = AVERAGE(Fact_Trips[trip_distance_miles])

# Average Trip Duration
AVG Trip Duration (Min) = 
AVERAGEX('Fact_Trips', DATEDIFF('Fact_Trips'[Pickup_date], 'Fact_Trips'[Dropoff_date], MINUTE))

# Connection quality
Store & Forward % = 
VAR StoreForwardCount = 
    CALCULATE(
        COUNTROWS('Fact_Trips'), 
        'Fact_Trips'[Store_forward_flag] = "store and forward trip" 
    )
VAR TotalTrips = COUNTROWS('Fact_Trips')

RETURN
    DIVIDE(StoreForwardCount, TotalTrips, 0)

# Number of passengers
Total Passenger Count = SUM(Fact_Trips[passenger_count])