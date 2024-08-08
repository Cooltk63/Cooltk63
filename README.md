//Sending Data from FrontEnd 
body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),

//Getting Data in Backend
Map<String, String> mapData = (Map<String, String>) map.get("data");

// Here i pass the map data to method
List<List<String>> result = crsStndAssetsRepository.getScreenDetails(mapData.get("submissionId"));

// Below is my Service.js code
let express = require("express");
let morgan = require('morgan');
let fetch = require('node-fetch');
const extractToken = require("../../Security/JwtMiddleware");
const {DataValidator, LegalRequest} = require("../../Security/RequstValidator");

let app = express();
app.use(morgan('dev'));

let router = express.Router();
module.exports = router;

/**
 * This is call for fetching report screen data from backend.
 * @author: V1014064
 * Response: {}*/
router.post('/tabData', extractToken, DataValidator, LegalRequest, async (req, res) => {
    let token = req.headers.authorization;
    console.log("after decryption :: ", req.data);
    if (!token) {
        return res.status(401).json({message: 'Unauthorized api call, request not from legal source'});
    }
    try {
        // const serviceUrl = 'http://10.0.26.232:7001/CRS/test/tabData';
        const serviceUrl = 'http://localhost:8081/RW02/getScreenDetails';
        console.log("Data Sending : ",typeof req.data)
        const response = await fetch(serviceUrl, {
            method: 'POST',
            body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': token
            },
        });
        const data = await response.json();
        res.json(data);
    } catch (e) {
        res.status(500).json({status: 'Failed to bring response, no connection between LB and APP'});
    }

})

/**
 * This is call for saving the screen data in a static screen.
 * @author: V1014064
 * Response: boolean*/
router.post('/staticSave', extractToken, DataValidator, LegalRequest, async (req, res) => {
    let token = req.headers.authorization;
    console.log("after decryption :: ", req.data);
    console.log(req);
    console.log('req.body.payload :: ',req.body.payload);
    if (!token) {
        return res.status(401).json({message: 'Unauthorized api call, request not from legal source'});
    }
    try {
        const serviceUrl = 'http://10.0.26.232:7001/CRS/test/staticSave';
        const response = await fetch(serviceUrl, {
            method: 'POST',
            body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': token
            },
        });
        const data = await response.json();
        console.log('response data :: '+data)
        res.json(data);
    } catch (e) {
        res.status(500).json({status: 'Failed to save the data, no connection between LB and APP'});
    }
})

/**
 * This is a call to save the row data i.e update the data in backend.
 * @author: V1014064
 * Response: boolean*/
router.post('/saveAddRow', extractToken, DataValidator, LegalRequest, async (req, res) => {
    let token = req.headers.authorization;
    console.log("after decryption :: ", req.data);
    console.log('length :: ',req.data.lengthOf);
    if (!token) {
        return res.status(401).json({message: 'Unauthorized api call, request not from legal source'});
    }
    try {
        const serviceUrl = 'http://10.0.26.232:7001/CRS/test/saveAddRow';
        const response = await fetch(serviceUrl, {
            method: 'POST',
            body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': token
            },
        });
        const data = await response.json();
        console.log('response data :: '+data)
        res.json(data);
    } catch (e) {
        res.status(500).json({status: 'Failed to save the data, no connection between LB and APP'});
    }
})

/**
 * This is call to send necessary data in order to delete the data in backend.
 * @author: V1014064
 * Response: boolean*/
router.post('/deleteAddRow', extractToken, DataValidator, LegalRequest, async (req, res) => {
    let token = req.headers.authorization;
    console.log("after decryption :: ", req.data);
    if (!token) {
        return res.status(401).json({message: 'Unauthorized api call, request not from legal source'});
    }
    try {
        const serviceUrl = 'http://10.0.26.232:7001/CRS/test/deleteAddRow';
        const response = await fetch(serviceUrl, {
            method: 'POST',
            body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': token
            },
        });
        const data = await response.json();
        console.log('response data :: '+data)
        res.json(data);
    } catch (e) {
        res.status(500).json({status: 'Failed to delete the data, no connection between LB and APP'});
    }
})

/**
 * This is call to send necessary data in order to delete the data in backend.
 * @author: V1014064
 * Response: boolean*/
router.post('/submitReport', extractToken, DataValidator, LegalRequest, async (req, res) => {
    let token = req.headers.authorization;
    console.log("after decryption :: ", req.data);
    if (!token) {
        return res.status(401).json({message: 'Unauthorized api call, request not from legal source'});
    }
    try {
        const serviceUrl = 'http://10.0.26.232:7001/CRS/test/submitReport';
        const response = await fetch(serviceUrl, {
            method: 'POST',
            body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': token
            },
        });
        const data = await response.json();
        console.log('response data :: '+data)
        res.json(data);
    } catch (e) {
        res.status(500).json({status: 'Failed to delete the data, no connection between LB and APP'});
    }
})

/**
 * This is call to send necessary data in order to delete the data in backend.
 * @author: V1014064
 * Response: boolean*/
router.post('/markAsNil', extractToken, DataValidator, LegalRequest, async (req, res) => {
    let token = req.headers.authorization;
    console.log("after decryption :: ", req.data);
    if (!token) {
        return res.status(401).json({message: 'Unauthorized api call, request not from legal source'});
    }
    try {
        const serviceUrl = 'http://10.0.26.232:7001/CRS/test/markAsNil';
        const response = await fetch(serviceUrl, {
            method: 'POST',
            body: JSON.stringify({
                user: req.user,
                data: req.data,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': token
            },
        });
        const data = await response.json();
        console.log('response data :: '+data)
        res.json(data);
    } catch (e) {
        res.status(500).json({status: 'Failed to delete the data, no connection between LB and APP'});
    }
})

Getting the typeof req.data is: String

How did i get the data at backend in Map as i defined earlier

