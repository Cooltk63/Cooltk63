import React, { useEffect, useState } from 'react';
import saveAs from 'file-saver';
import './style.css';
import axios from 'axios';

const SalesData = () => {
    const [rows, setRows] = useState([]);
    const [salesPersons, setSalesPersons] = useState([]);
    const [nextId, setNextId] = useState(1);
    const [isAddDisabled, setIsAddDisabled] = useState(false);
    const [isSaveDisabled, setIsSaveDisabled] = useState(true);
    const [isSubmitDisabled, setIsSubmitDisabled] = useState(true);
    const [currentPage, setCurrentPage] = useState(1);
    const rowsPerPage = 6;

    const previousYearSale = 2000;
    const prevCust = 50;

    useEffect(() => {
        axios.get('https://reqres.in/api/users?page=2')
            .then(response => {
                if (Array.isArray(response.data.data)) {
                    const SalesPersonsNames = response.data.data.map(person => ({
                        id: person.id,
                        name: `${person.first_name} ${person.last_name}`
                    }));
                    setSalesPersons(SalesPersonsNames);
                }
                console.log("Received Data :" + JSON.stringify(response.data));
            })
            .catch(error => {
                alert("error while fetching api data :" + error);
            });
    }, []);

    useEffect(() => {
        const currentPageRows = rows.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage);
        if (currentPageRows.length === 0) {
            setIsAddDisabled(false);
        }
        if (validateRows()) {
            setIsSaveDisabled(false);
            setIsSubmitDisabled(false);
        } else {
            setIsSaveDisabled(true);
            setIsSubmitDisabled(true);
        }
    }, [rows]);

    const submiData = () => {
        const fileName = 'Sales_Data.txt';
        const fileContent = JSON.stringify(rows, null, 2);
        const blob = new Blob([fileContent], { type: 'text/plain;charset=utf-8' });
        saveAs(blob, fileName);
        alert("Data Submitted Successfully");
    }

    const addRow = () => {
        const currentPageRows = rows.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage);
        if (!validateRows() && currentPageRows.length !== 0) {
            alert("Please fill all data fields before adding a new row");
            return;
        }

        if (currentPageRows.length >= rowsPerPage) return;

        setRows(prevRows => [...prevRows, {
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
        setNextId(prevNextId => prevNextId + 1);
        setIsAddDisabled(true);
    };

    const deleteRow = (orderId) => {
        const updatedRows = rows.filter(row => row.orderId !== orderId);
        setRows(updatedRows);
        setIsAddDisabled(updatedRows.length === 0 || !validateRows(updatedRows));
    };

    const handleInputChange = (orderId, event) => {
        const { name, value } = event.target;
        setRows(prevRows => prevRows.map(row => {
            if (row.orderId === orderId) {
                const updatedRow = { ...row };
                updatedRow[name] = value;
                if (name === "quantity" || name === "rate") {
                    const quantity = name === "quantity" ? parseFloat(value) : parseFloat(row.quantity);
                    const rate = name === "rate" ? parseFloat(value) : parseFloat(row.rate);
                    updatedRow.salesValue = (quantity * rate).toFixed(2);
                }
                return updatedRow;
            }
            return row;
        }));
        setIsAddDisabled(!validateRows());
    };

    const validateRows = () => {
        const currentPageRows = rows.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage);
        for (let row of currentPageRows) {
            if (!row.customerName || !row.productName || !row.quantity || !row.rate || !row.salesDate || !row.salesPersonName || !row.status) {
                setIsSaveDisabled(true);
                return false;
            }
        }
        setIsSaveDisabled(false);
        return true;
    };

    const handlePageChange = (direction) => {
        if (direction === 'next' && currentPage < Math.ceil(rows.length / rowsPerPage)) {
            setCurrentPage(currentPage + 1);
        } else if (direction === 'prev' && currentPage > 1) {
            setCurrentPage(currentPage - 1);
        }
    };

    const currentPageRows = rows.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage);

    const saveData = (orderId) => {
        alert(`Data for Order ID ${orderId} Saved Successfully`);
    }

    const calculateTotalSales = () => {
        return rows.reduce((total, row) => total + (parseFloat(row.salesValue) || 0), 0);
    };

    const calculateTotalCustomers = () => {
        const customersQty = new Set(rows.map(row => row.customerName));
        return customersQty.size;
    };

    const totalCustomers = calculateTotalCustomers();
    const custDataPercentage = ((totalCustomers - prevCust) / prevCust * 100).toFixed(2);

    const totalSales = calculateTotalSales();
    const percentageValue = ((totalSales - previousYearSale) / previousYearSale * 100).toFixed(2);

    return (
        <div className="section">
            <div className="CardData">
                <div className="Card">
                    <h1>Total Customers</h1>
                    <p>{totalCustomers}</p>
                    <p>{custDataPercentage}% Declined</p>
                </div>
                <div className="Card">
                    <h1>Total Sales</h1>
                    <p>Rs.{totalSales}</p>
                    <p>{percentageValue}% Total Growth</p>
                </div>
            </div>

            <div className="CardDatas">
                <div className="Cards">
                    <input type="search" id="searchbox" name="searchbox" placeholder="search" />
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
                        {currentPageRows.map((row) => (
                            <tr key={row.orderId}>
                                <td><input type="checkbox" checked={row.checked} readOnly /></td>
                                <td>{row.orderId}</td>
                                <td><input type="text" value={row.customerName} onChange={(e) => handleInputChange(row.orderId, e)} name="customerName" /></td>
                                <td><input type="text" value={row.productName} onChange={(e) => handleInputChange(row.orderId, e)} name="productName" /></td>
                                <td><input type="number" value={row.quantity} onChange={(e) => handleInputChange(row.orderId, e)} name="quantity" /></td>
                                <td><input type="number" value={row.rate} onChange={(e) => handleInputChange(row.orderId, e)} name="rate" /></td>
                                <td>{row.salesValue}</td>
                                <td><input type="date" value={row.salesDate} onChange={(e) => handleInputChange(row.orderId, e)} name="salesDate" /></td>
                                <td>
                                    <select value={row.salesPersonName} onChange={(e) => handleInputChange(row.orderId, e)} name="salesPersonName">
                                        <option value="">Select Sales Person</option>
                                        {salesPersons.map(person => (
                                            <option key={person.id} value={person.name}>{person.name}</option>
                                        ))}
                                    </select>
                                </td>
                                <td><input type="text" value={row.status} onChange={(e) => handleInputChange(row.orderId, e)} name="status" /></td>
                                <td>
                                    <button className="btn btn-outline-primary modal-dialog-centered" id="saveBtn"
                                            onClick={() => saveData(row.orderId)}
                                            disabled={isSaveDisabled}>Save
                                    </button>
                                </td>
                                <td>
                                    <button className="delBtn" id="delBtn"
                                            onClick={() => deleteRow(row.orderId)}>Delete
                                    </button>
