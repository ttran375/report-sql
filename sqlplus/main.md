``` python
import cx_Oracle

userpwd = "password"  # Obtain password string from a user prompt or environment variable

connection = cx_Oracle.connect(
    user="COMP214_W24_nic_35",
    password=userpwd,
    dsn="199.212.26.208:1521/SQLD",
    encoding="UTF-8"
)

# Connection established successfully, proceed with your database operations
# Create cursor
cursor = connection.cursor()
# enable DBMS_OUTPUT
cursor.callproc("dbms_output.enable")
```

    ## []

``` python
# execute some PL/SQL that calls DBMS_OUTPUT.PUT_LINE
cursor.execute("""
DECLARE 
    v_qty NUMBER;
    v_ship NUMBER;
BEGIN 
    v_qty := 15; -- Set the quantity
    SHIP_COST_SP(v_qty, v_ship); -- Call the procedure
    DBMS_OUTPUT.PUT_LINE('Shipping cost for quantity ' || v_qty || ' is: ' || v_ship);
END;
""")

# Fetch the output
statusVar = cursor.var(cx_Oracle.NUMBER)
lineVar = cursor.var(cx_Oracle.STRING)
while True:
    cursor.callproc("dbms_output.get_line", (lineVar, statusVar))
    if statusVar.getvalue() != 0:
        break
    print(lineVar.getvalue())
```

    ## ['Shipping cost for quantity 15 is: 11', 0.0]
    ## Shipping cost for quantity 15 is: 11
    ## [None, 1.0]
