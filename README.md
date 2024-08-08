//Getting Data in Backend
Map<String, String> mapData = (Map<String, String>) map.get("data");

//Sending Data from FrontEnd 
body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),

