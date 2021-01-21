
import React, { useState } from 'react';
import './bookmarks.css';
import { Sortable } from '@progress/kendo-react-sortable';
import { Button, Modal } from 'react-bootstrap';

export class BookMark extends React.Component {
    tags = { 'books': 'books', 'electronics': 'electronics', 'clothes': 'clothes', 'vehicles': 'vehicles', 'vessels': 'vessels' }
    dataArray = [
        { id: 1, text: 'www.shakespeare.com', tag: this.tags.books },
        { id: 2, text: 'www.electronics.com', tag: this.tags.electronics },
        { id: 3, text: 'www.clothes.com', tag: this.tags.clothes },
        { id: 4, text: 'www.homeappliances.com', tag: this.tags.vessels },
        { id: 5, text: 'www.scooty.com', tag: this.tags.vehicles },
        { id: 6, text: 'www.reacttutorial.com', tag: this.tags.books },
    ]
    state = {
        data: [
            ...this.dataArray
        ],
        events: [],
        url: '',
        searchInput: '',
    };
    element; dataArray;
    render() {
        return (
            <div className="container-fluid">
                <Modal.Dialog >
                    <Modal.Header>
                        <Modal.Title>Add Tags to the Link</Modal.Title>
                    </Modal.Header>
                    <Modal.Body>
                        <div className="modal-tag">
                            <label >Tag Name </label>
                            <select onChange={this.addTag} name="tagName" id="tagName">
                                <option>Select the tag</option>
                                <option value={this.tags.books}>{this.tags.books}</option>
                                <option value={this.tags.vessels}>{this.tags.vessels}</option>
                                <option value={this.tags.vehicles}>{this.tags.vehicles}</option>
                                <option value={this.tags.clothes}>{this.tags.clothes}</option>
                                <option value={this.tags.electronics}>{this.tags.electronics}</option>
                            </select>
                        </div>
                    </Modal.Body>
                    <Modal.Footer>
                        <Button variant="secondary" onClick={this.cancel}>cancel</Button>
                        <Button variant="primary" onClick={this.ok}>Ok</Button>
                    </Modal.Footer>
                </Modal.Dialog>

                <div className="row ">
                    <div className ="col-12 bookmark">
                        <h1> Manage Bookmarks</h1>
                        </div>
                    </div>
                <div className="row urlWrapper">
                    <div className="col-lg-8 col-md-8 col-sm-8" id="url">
                        <input onChange={this.changeHandler} type="text" placeholder="Enter any url"></input>
                    </div>
                    <div className="col-lg-3 col-md-3 col-sm-3" >
                        <button id="add" onClick={this.addBookmarkUrl}>Add</button>
                    </div>

                </div>
                <div className="section">
                    <div className="row align-items-end">
                        <div className="border col-lg-12 col-md-12 col-sm-12">
                            <div className="float-left col-lg-4 col-md-4 col-sm-4">
                                <select onChange={this.changeTags} name="tags" id="tags">
                                    <option>Select the tag</option>
                                    <option value={this.tags.books}>{this.tags.books}</option>
                                    <option value={this.tags.vessels}>{this.tags.vessels}</option>
                                    <option value={this.tags.vehicles}>{this.tags.vehicles}</option>
                                    <option value={this.tags.clothes}>{this.tags.clothes}</option>
                                    <option value={this.tags.electronics}>{this.tags.electronics}</option>
                                </select>
                            </div>
                            <div className="float-right col-lg-4 col-md-4 col-sm-4">
                                <input className="searchInput" onChange={this.search} type="text" placeholder="search"></input>
                            </div>
                        </div>
                    </div>
                </div>
                <Sortable
                    idField={'id'}
                    disabledField={'disabled'}
                    data={this.state.data}
                    itemUI={SortableItemUI}
                    onDragStart={this.onDragStart}
                    onDragOver={this.onDragOver}
                    onDragEnd={this.onDragEnd}
                    onNavigate={this.onNavigate}
                />
                <div className={'example-config'} style={{ marginTop: 20 }}>
                    <ul className={'event-log'} ref={this.assignRef}>
                    </ul>
                </div>
            </div>
        );
    }
    componentDidUpdate() {
        this.element.scrollTop = this.element.scrollHeight;
    }
    addBookmarkUrl = (event) => {
        if (this.state.url != '') {
            document.getElementsByClassName("modal-dialog")[0].style.display = "block";
        }
    }
    changeHandler = (event) => {
        console.log(event.target.value);
        this.setState({ url: event.target.value })
        console.log(this.state.url, 'url')
    }
    search = (event) => {
        this.setState({ searchInput: event.target.value });
        this.setState({ data: this.dataArray })
        let count = 0;
        let tagArray = []
        for (var i = 0; i < this.dataArray.length; i++) {
            if (this.dataArray[i].text.indexOf(event.target.value) != -1) {
                count++;
                tagArray.push(this.dataArray[i])
                this.setState({ data: tagArray });
            }
        }
    }
    changeTags = (event) => {
        console.log(event.target.value, 'value');
        this.setState({ data: this.dataArray })
        let count = 0;
        let tagArray = []
        for (var i = 0; i < this.dataArray.length; i++) {
            if (event.target.value.indexOf(this.dataArray[i].tag) != -1) {
                count++;
                tagArray.push(this.dataArray[i])
                this.setState({ data: tagArray });
            }
        }
    }
    addTag = (event) => {
        this.setState({ tagName: event.target.value })
        console.log(this.state.tagName, 'tagname');
    }
    onDragStart = (event) => {
        this.setState({
            events: [
                ...this.state.events,
                `Drag start: previous index:${event.prevIndex}`
            ]
        });
    }

    onDragOver = (event) => {
        this.setState({
            data: event.newState,
            events: [
                ...this.state.events,
                `Drag over: previous index:${event.prevIndex}, next index: ${event.nextIndex}`
            ]
        });
    }

    onDragEnd = (event) => {
        this.setState({
            events: [
                ...this.state.events,
                `Drag end: previous index:${event.prevIndex}, next index: ${event.nextIndex}`
            ]
        });
    }
    onNavigate = (event) => {
        this.setState({
            data: event.newState,
            events: [
                ...this.state.events,
                `Keyboard navigation: previous index:${event.prevIndex}, next index: ${event.nextIndex}`
            ]
        });
    }

    assignRef = (element) => {
        this.element = element;
    }
    ok = (event) => {
        document.getElementsByClassName("modal-dialog")[0].style.display = "none";
        var lastIndex = this.state.data.length;
        this.dataArray.push({ id: lastIndex + 1, text: this.state.url, tag: this.state.tagName })
        console.log(this.dataArray, 'dataarray')
        var joined = this.state.data.concat({ id: lastIndex + 1, text: this.state.url, tag: this.state.tagName });
        this.setState({ data: joined });
    }
    cancel = () => {
        document.getElementsByClassName("modal-dialog")[0].style.display = "none";

    }
}

const getBaseItemStyle = (isActive) => ({
    fontSize: '16px',
    textAlign: 'center',
    outline: 'none',
    border: '1px solid',
    cursor: 'move',
    background: 'silver',
    color: isActive ? '#fff' : 'black',
    borderColor: 'darkgrey',
    padding: '10px',
    margin: '10px'
});

const SortableItemUI = (props) => {
    const { isDisabled, isActive, style, attributes, dataItem, forwardRef } = props;
    const classNames = ['col-xs-6 col-sm-3 bookmark'];

    if (isDisabled) {
        classNames.push('k-state-disabled');
    }

    return (
        <div
            ref={forwardRef}
            {...attributes}
            style={{
                ...getBaseItemStyle(isActive),
                ...style
            }}
            className={classNames.join(' ')}
        >
            <span className="tags">{dataItem.tag}</span><br />
            <span className="text">{dataItem.text}</span>
        </div>
    );
};

.container-fluid{
    margin-top:50px;
}
  .urlWrapper, .section{
      margin: 20px;
  }
  #url input, #add, .searchInput{
      padding:10px;
      width:100%;
      font-size:14px !important;
  }
  #add{border-radius:20px;}
  .border{
      border:1px solid #000;
      padding:10px;
  }
  .tags{
      color:#fff;
      font-size:12px;
      background-color: #777;
      border-radius: 20px;
      margin-top:10px;
      padding: 5px 10px;
     float: right;
     font-style: italic;
  }
  .text{
      font-size:14px;
     display:inline-block;
     padding-top:20px;
  }
 select#tags{
    width: 100%;
    padding: 6px;
    font-size: 14px;
 }
 select#tagName{
    padding: 6px;
    font-size: 14px;
 }
 .modal-dialog{
     position:absolute;
     width:50%;
     left:25%;
     top:25%;
     z-index:100;
     display:none;
 }
 .modal-tag{
     font-size:14px;
     margin-left:10%;
 }
 .modal-tag select{
     margin-left:10px;
 }
 .modal-tag h4{
     text-align:center;
 }
 .bookmark{
     text-align:center;
     padding-bottom:20px;
 }
<BrowserRouter>
  <Route path = "/bookmark" component = {BookMark} />
 </BrowserRouter>
