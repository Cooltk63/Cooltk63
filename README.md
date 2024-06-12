import React, {useEffect, useState} from 'react';
import saveAs from 'file-saver';
import './style.css';

import axios from 'axios';
import {type} from "@testing-library/user-event/dist/type";
import {toBeChecked} from "@testing-library/jest-dom/dist/to-be-checked";

const SalesData = () => {
    const [rows, setRows] = useState([]);
    const [salesPersons, setSalesPersons] = useState([]);
    const [nextId, setNextId] = useState(1);
    const [isAddDisabled, setIsAddDisabled] = useState(false);
    const [isSaveDisabled, setIsSaveDisabled] = useState(true);
    const [currentPage, setcurrentPage] = useState(1);
    const rowsPerPage=6;


    useEffect(() => {

        axios.get('https://reqres.in/api/users?page=2')
            .then(responce => {
                if (Array.isArray(responce.data.data)) {
                    const SalesPersonsNames = responce.data.data.map(person => ({
                        id: person.id,
                        name: `${person.first_name} ${person.last_name}`
                    }));
                    setSalesPersons(SalesPersonsNames);
                }
                console.log("Received Data :" + JSON.stringify(responce.data));

            })
            .catch(error => {
                alert("error while fetching api data :", error);
            })
    }, []);

    useEffect(()=>{
        setIsSaveDisabled(!validateRows());
    },[rows]);

    const submiData = () => {
        const fileName = 'Sales_Data.txt';
        const fileContent = JSON.stringify(rows, null, 2);
        const blob = new Blob([fileContent], {type: 'text/plain;charset=utf-8'});
        saveAs(blob, fileName);
    }

    const addRow = () => {
        if(!validateRows())
        {
            alert("Please fill the all data Fields before adding new Row");
            return;
        }
        const currentPageRows=rows.slice((currentPage -1)* rowsPerPage,currentPage * rowsPerPage);
        if(currentPageRows.length>=rowsPerPage)return;


        // setRows(rows.concat({
        setRows(prevRows =>[...prevRows,
            {
            orderId: nextId,
            checked: false,
            customerName: '',
            productName: '',
            quantity: '',
            rate: '',
            salesValue: '',
            salesDate: '',
            salesPersonName: '',
            status: '',
        }]);
        setNextId(prevNextId=>prevNextId+1);
        setIsAddDisabled(true);
    };

    const deleteRow = (orderId) => {
        const updatedRows=rows.filter(row=>row.orderId !== orderId);
        setRows(updatedRows);
        setIsAddDisabled(updatedRows.length === 0 || !validateRows(updatedRows));
    };


    const handleInputChange = (orderId, event) => {
        const {name, value} = event.target;
        setRows(prevRows => prevRows.map(row => {
            if (row.orderId === orderId) {
                const updatedRow = {
                    ...row
                };
                if (type === "checkbox") {
                    updatedRow[name] = toBeChecked;
                } else {
                    updatedRow[name] = value;
                }
                if (name === "quantity" || name === "rate") {
                    const quantity = name === "quantity" ? parseFloat(value) : parseFloat(row.quantity);
                    const rate = name === "rate" ? parseFloat(value) : parseFloat(row.rate);
                    updatedRow.salesValue = (quantity * rate).toFixed(2);
                }
                // updatedRow[name] = value;
                return updatedRow;
            }
            return row;
        }));
            setIsAddDisabled(!validateRows());
            setIsSaveDisabled(!validateRows());

    };

    const validateRows=()=>
    {
        const curretPageRows=rows.slice(
            (currentPage-1) * rowsPerPage, currentPage* rowsPerPage);
        for(let row of curretPageRows)
            if(!row.customerName ||!row.productName || !row.quantity|| !row.rate||!row.salesDate ||!row.salesPersonName||!row.status)
            {
                return false;
            }
        return true;
    };

    const handlePageChnage=(direction)=>
    {
        if(direction === 'next' && currentPage >1)
        {
            setcurrentPage(currentPage +1);
        }else if(direction === 'prev' && currentPage>1) {
        setcurrentPage(currentPage-1);
    }

    };
    const currentPageRows=rows.slice((currentPage-1)* rowsPerPage,currentPage* rowsPerPage);

    const saveData = () => {
        alert("Data Saved Successfully");
    }

    return (
        <div className="section">
            <div className="CardData">
                <div className="Card">
                    <h1> Cal Data </h1>
                </div>

                <div className="Card">
                    <h1> Cal Data </h1>
                </div>
            </div>

            <div className="CardDatas">
                <div className="Cards">
                    <input type="search" id="searchbox" name="searchbox" placeholder="serach"/>
                </div>

                <div className="Cards">
                    <button onClick={addRow} disabled={isAddDisabled}>Add Row</button>
                </div>
            </div>


            <div className="container">
                <table className="tables2">

                    <thead>
                    <tr>
                        <td>Select Id</td>
                        <td>Order Id</td>
                        <td>Customer Name</td>
                        <td>Product Name</td>
                        <td>Quantity</td>
                        <td>Rate</td>
                        <td>Sales Value</td>
                        <td>Date of Sales</td>
                        <td>Sales Person</td>
                        <td>Status</td>
                    </tr>
                    </thead>
                    <tbody className="table-body">
                    {rows.map((row) =>
                        (
                            <tr key={row.orderId}>
                                <td><input type="checkbox" checked={row.checked}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="checked"/></td>
                                <td>{row.orderId}</td>
                                <td><input type="text" value={row.customerName}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="customerName"/>
                                </td>
                                <td><input type="text" value={row.productName}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="productName"/></td>
                                <td><input type="number" value={row.quantity}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="quantity"/></td>
                                <td><input type="number" value={row.rate}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="rate"/></td>
                                <td>{row.salesValue}</td>
                                <td><input type="date" value={row.salesDate}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="salesDate"/></td>
                                <td>
                                    <select value={row.salesPersonName}
                                            onChange={(e) => handleInputChange(row.orderId, e)} name="salesPersonName">
                                        <option value="">Select Sales Person</option>
                                        {Array.isArray(salesPersons) && salesPersons.map(person => (
                                            <option key={person.id} value={person.name}></option>))}
                                    </select></td>
                                <td><input type="text" value={row.status}
                                           onChange={(e) => handleInputChange(row.orderId, e)} name="status"/></td>
                                <td>
                                    <button className="saveBtn" onClick={() => saveData(row.orderId)}>Save</button>
                                </td>

                                <td>
                                    <button className="delBtn" onClick={(deleteR) => deleteRow(row.orderId)}>Delete
                                    </button>
                                </td>
                            </tr>
                        ))}
                    </tbody>
                </table>
            </div>
            <div className="pagination">
                <button onClick={()=>handlePageChnage('prev')} disabled={currentPage===1}>Previous</button>
                <span> Page{currentPage}</span>
                <button onClick={()=>handlePageChnage('next')} disabled={currentPage>= Math.floor(rows.length-1 /rowsPerPage)+1}>Next</button>
            </div>
            <div className="footer">
                <button className="submitBtn" type="submit" onClick={submiData}> Submit</button>
            </div>
        </div>
    )
}

export default SalesData
