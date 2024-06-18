// Handle search input change
    const handleSearchChange = (event) => {
        setSearchQuery(event.target.value);
    };

    // Filter rows based on search query
    const filteredRows = rows.filter(row => 
        row.customerName.toLowerCase().includes(searchQuery.toLowerCase()) ||
        row.productName.toLowerCase().includes(searchQuery.toLowerCase()) ||
        row.salesPersonName.toLowerCase().includes(searchQuery.toLowerCase())
    );

    const filteredPageRows = filteredRows.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage);

    retur
