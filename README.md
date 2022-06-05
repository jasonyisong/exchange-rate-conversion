# Exchange-Rate-Conversion

1. First, you need to create a free account on abstractapi.com.
<img width="369" alt="image" src="https://user-images.githubusercontent.com/33503189/172073654-cf5d4f55-b614-4676-874d-6624d88fd7bf.png">

3. Click Exchange rates on the left menu, and get your free API key in the right side.
<img width="369" alt="image" src="https://user-images.githubusercontent.com/33503189/172073659-8f2df6c5-b307-4b9c-8492-5a30fb0f8912.png">

5. PL/SQL code in Oracle APEX. You can call with DA or written as functions, api_key better best saved in the database table, so easy to manage.
、、、

declare
    l_clob           clob;
    l_exchange_rate  number;
    l_api_key        varchar2(200);
begin
    -- Reference: https://www.abstractapi.com/exchange-rate-api
    -- Use the get method to request the API to get the exchange rate with specific date, from base USD to target CNY
    l_api_key:='your api key';
    l_clob := apex_web_service.make_rest_request(
        p_url => 'https://exchange-rates.abstractapi.com/v1/convert/?api_key='||l_api_key||'&base=USD&target=CNY&date='||:P19_EXCHANGE_DATE,
        p_http_method => 'GET' );

    -- Parse the returned json data
    APEX_JSON.parse(l_clob);  

    -- Get Exchange Rate value from the returned json data content
    l_exchange_rate := APEX_JSON.get_varchar2(p_path => 'exchange_rate');

    -- Conversion of contract USD amount to CNY amount, and return back with this Dynamic Action 
    :P19_CNY_AMT:=l_exchange_rate*:P19_AMT;
end;

、、、
