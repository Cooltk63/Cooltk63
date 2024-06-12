.section {
    padding: 1px;
    width: auto;
    background: #222A35;
    color: #ffffff;
    height: 100%;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.CardData {
    margin-top: 10px;
    display: flex;
    align-items: center;
    color: #ffffff;
    justify-content: space-evenly;
}

.Card {
    color: #ffffff;
    border: 2px solid rgba(0, 0, 0, .2);
    box-shadow: 0 0 10px rgba(255, 255, 255, .2);
}

.CardDatas {
    padding-top: 10px;
    margin-top: 10px;
    margin-bottom: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    color: #ffffff;
    padding-left: 10px;
    padding-right: 10px;
}

.CardDatas #searchbox {
    width: 12em;
    height: 2.5em;
    background: #ffffff;
    border: none;
    outline: none;
    border-radius: 40px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .1);
    cursor: pointer;
    font-size: 14px;
    color: #333;
    font-weight: 700;
}

.CardDatas button {
    width: 12em;
    height: 2.5em;
    background: #ffffff;
    border: none;
    outline: none;
    border-radius: 40px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .1);
    cursor: pointer;
    font-size: 14px;
    color: #333;
    font-weight: 700;
}

.CardDatas #searchbox::placeholder {
    text-align: center;
    color: #575C66;
    margin-left: 2px;
}

.search {
    box-shadow: 0 0 10px rgba(255, 255, 255, .5);
}

.container {
    color: #575C66;
    margin-top: 20px;
    margin-bottom: 20px;
    height: 20em;
    width: auto;
    border-radius: 10px;
    padding-right: 5px;
    padding-left: 5px;
    overflow-y: auto;
    max-height: 400px;
}

input:focus, select:focus {
    outline: none;
    border: 3px solid rgba(73, 222, 14, 0.2);
}

.tables2 td {
    margin-top: 5px;
    padding: 2px;
    border: 1px solid grey;
}

.tables2 td input {
    border: none;
    box-sizing: border-box;
}

.tables2 td select {
    border: none;
    box-sizing: border-box;
}

.table-body input::placeholder {
    color: #ffff;
}

select option {
    color: #000;
    background-color: #ffffff;
    padding: 8px;
}

.saveBtn, .delBtn, .submitBtn, .pagination button {
    width: 12em;
    height: 2.5em;
    background: #ffffff;
    border: none;
    outline: none;
    border-radius: 40px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .1);
    cursor: pointer;
    font-size: 14px;
    color: #333;
    font-weight: 700;
}

.pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 10px 0;
}

.footer {
    display: flex;
    justify-content: center;
    width: 100%;
    padding: 10px 0;
}

.submitBtn {
    margin-top: 20px;
}
