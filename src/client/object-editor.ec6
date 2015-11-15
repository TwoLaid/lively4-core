"use strict";

/*var _HTMLElement = HTMLElement;
function HTMLElement() {}
HTMLElement.prototype = _HTMLElement.prototype;*/

export class WindowElement extends HTMLDivElement {

  // window title
  get title() {
    return this._title;
  }
  set title(val) {
    this._title =  val;
    this.render();
  }

  // content of the window body
  get content() {
    return this._content;
  }
  set content(val) {
    this._content =  val;

    if (this.created) {
      this.contentBlock.innerHTML = val;
    }
  }

  // additional CSS styles
  get styles() {
    return this._styles;
  }
  set styles(val) {
    this._styles =  val;

    if (this.created) {
      this.stylesBlock.innerHTML = val;
    }
  }

  // called when element is created
  createdCallback() {
    console.log('window createdCallback!');

    this.createShadowRoot().innerHTML = `
      <style>
        :host * {
            box-sizing: border-box;
        }
        .window {
            width: auto;
            height: auto;
            display: block;
            background-color: #ffffff;
            box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.3);
        }
        .window-titlebar {
            position: relative;
            display: flex;
            width: 100%;
            padding: 0.3em 0.4em;
            border: none;
            border-bottom: 1px #e9e9e9 solid;
        }
        .window-title {
            flex-grow: 1;
            font-weight: 600;
            vertical-align: middle;
            cursor: move;
        }
        .window-title span {
            vertical-align: middle;
            font-size: 1.1em;
            line-height: 1.5rem;
        }
        .window-controls {
            flex-grow: 0;
        }
        .window-min, .window-max, .window-close {
            display: inline-block;
            color: #777777;
            background-color: #ffffff;
            width: 1.5rem;
            text-align: center;
            margin-left: 1px;
            font-weight: 700;
            cursor: pointer;
            line-height: 1.5rem;
        }
        .window-min:hover, .window-max:hover, .window-close:hover {
            background-color: #cde6f7;
            color: #2a8dd4;
        }
        .window-content {
            display: block;
            position: relative;
            padding: 0.4em;
            width: 100%;
            height: auto;
        }
      </style>

      <style id="window-styles">
      </style>

      <div class="window">
          <div class="window-titlebar">
              <div class="window-title"><span></span></div>
              <div class="window-controls">
                  <span class="window-min">&lowbar;</span>
                  <span class="window-max">&#9634;</span>
                  <span class="window-close">&#10005;</span>
              </div>
          </div>
          <div class="window-content" id="window-content"></div>
      </div>
    `;

    // define shortcut variables
    this.titleSpan = this.shadowRoot.querySelector('.window-title span');
    this.minButton = this.shadowRoot.querySelector('.window-min');
    this.maxButton = this.shadowRoot.querySelector('.window-max');
    this.closeButton = this.shadowRoot.querySelector('.window-close');
    this.contentBlock = this.shadowRoot.querySelector('#window-content');
    this.stylesBlock = this.shadowRoot.querySelector('#window-styles');

    // bind events for window behavior
    this.dragging = false;
    this.closeButton.addEventListener('click', (e) => { this.closeButtonClicked(e) });
    this.minButton.addEventListener('click', (e) => { this.minButtonClicked(e) });
    this.maxButton.addEventListener('click', (e) => { this.maxButtonClicked(e) });
    this.shadowRoot.querySelector('.window-title').addEventListener('mousedown', (e) => { this.titleMouseDown(e) });
    document.body.addEventListener('mousemove', (e) => { this.windowMouseMove(e) });
    document.body.addEventListener('mouseup', (e) => { this.windowMouseUp(e) });

    this.created = true;
    this.render();
  }

  attachedCallback() {
    console.log('window attachedCallback!');
  }

  // (re)render the element
  render() {
    console.log('window render!');
    if (this.created) {
      this.titleSpan.innerText = this.title;
      this.stylesBlock.innerHTML = this.styles;

      // only update when it changed, so we dont update it unnecessarily
      if (this.contentBlock.innerHTML !== this.content) {
        this.contentBlock.innerHTML = this.content;
      }
    }
  }

  minButtonClicked(e) {
    // TODO
  }

  maxButtonClicked(e) {
    // TODO
  }

  closeButtonClicked(e) {
    this.parentNode.removeChild(this);
  }

  titleMouseDown(e) {
    let offsetWindow = $(this).offset();

    this.dragging = {
        left: e.pageX - offsetWindow.left,
        top: e.pageY - offsetWindow.top
    };
  }

  windowMouseUp(e) {
    this.dragging = false;
  }

  windowMouseMove(e) {
    if (this.dragging) {
      $(this).css({
          left: e.pageX - this.dragging.left,
          top: e.pageY - this.dragging.top
      });
    }
  }
}

export class ObjectEditor extends WindowElement {

  get target() {
    return this._target;
  }

  set target(val) {
    this._target = val;
    this.title = val.tagName;
    this.render();
  }

  createdCallback() {
    console.log('editor createdCallback!');

    let styles = `
      :host {
        display: block;
        position: fixed;
        top: calc(50% - 150px);
        left: calc(50% - 200px);
        right: 50%;
        bottom: 50%;
        background: white;
        width: 400px;
        height: 300px;
        z-index: 100;
      }

      .main-content {
          width: 100%;
          display: flex;
      }
      #script-list {
          width: 20%;
          min-width: 100px;
          padding-right: 2px;
      }
      #script-list select {
          width: 100%;
          height: 100%;
      }
      textarea {
          width: 80%;
      }
      .toolbar {
          margin-top: 0.4rem;
      }
      .toolbar .bar-left {
          display: inline-block;
          text-align: left;
      }
      .toolbar .bar-right {
          float: right;
          display: inline-block;
          text-align: right;
      }
    `;

    let content = `
      <div class="main-content">
          <div id="script-list">
              <select size="20"></select>
          </div>
          <textarea id="editor"></textarea>
      </div>

      <div class="toolbar">
        <div class="bar-left">
          <button id="add-script">Add</button>
          <button id="remove-script">Remove</button>
        </div>
        <div class="bar-right">
          <button id="save-script">Save</button>
        </div>
      </div>
    `;

    this.styles = styles;
    this.content = content;

    super.createdCallback();
  }

  attachedCallback() {
    console.log('editor attachedCallback!');
    this.attached = true;

    this.scriptList = this.shadowRoot.querySelector('#script-list select');
    this.editor = this.shadowRoot.querySelector('#editor');
    this.addButton = this.shadowRoot.querySelector('#add-script');
    this.removeButton = this.shadowRoot.querySelector('#remove-script');
    this.saveButton = this.shadowRoot.querySelector('#save-script');

    this.scriptList.addEventListener('change', (e) => { this.scriptChanged(e) });
    this.addButton.addEventListener('click', (e) => { this.addButtonClicked(e) });
    this.removeButton.addEventListener('click', (e) => { this.removeButtonClicked(e) });
    this.saveButton.addEventListener('click', (e) => { this.saveButtonClicked(e) });

    this.render();
  }

  render() {
    super.render();
    console.log('editor render!');
    if (!this.attached) {
      return;
    }

    // Scripts
    this.updateScripts();
  }

  addButtonClicked(e) {
    if (!this.target) {
      return;
    }

    if (typeof this.target.__scripts__ === 'undefined') {
        this.target.__scripts__ = {};
    }

    var scriptName = prompt('Please enter a new script name', '');
    if (!scriptName || scriptName.length <= 0) {
        return;
    }

    if (typeof this.target.__scripts__[scriptName] !== 'undefined') {
        alert('Script name already in use.');
        return;
    }

    scriptManager.addScript(this.target, 'function ' + scriptName + '() {\n  \n}');
    this.updateScripts();
    this.scriptList.selectedIndex = Object.keys(this.target.__scripts__).length - 1;
    this.scriptChanged();
  }

  removeButtonClicked(e) {
    if (this.target) {
      let scriptName = this.scriptList.value;
      if (scriptName) {
        delete this.target.__scripts__[scriptName];
      }
      this.editor.value = '';
      this.scriptList.removeChild(this.scriptList.querySelector('option[value="' + scriptName + '"]'));
    }
  }

  saveButtonClicked(e) {
    if (this.target && this.scriptList.selectedIndex >= 0) {
      var scriptName = scriptList.selectedOptions[0].value;
      scriptManager.addScript(this.target, this.editor.value, { update: true });
    }
  }

  scriptChanged(e) {
    let scriptList = this.shadowRoot.querySelector('#script-list select');
    if (scriptList.selectedIndex >= 0) {
      var scriptName = scriptList.selectedOptions[0].value;
      this.loadScript(scriptName);
    }
  }

  loadScript(scriptName) {
    if (typeof this.target.__scripts__ === 'undefined' ||
      typeof this.target.__scripts__[scriptName] === 'undefined') {
      return;
    }

    // this.editor.innerText = this.target.__scripts__[scriptName];
    this.shadowRoot.querySelector('#editor').innerText = this.target.__scripts__[scriptName];
  }

  updateScripts() {
    let scriptHtml = '';
    if (this.target && typeof this.target.__scripts__ !== 'undefined') {
      for (let scriptName in this.target.__scripts__) {
        scriptHtml += '<option value="' + scriptName + '">' + scriptName + '</option>';
      }
    }
    this.shadowRoot.querySelector('#script-list select').innerHTML = scriptHtml;
  }
}

document.registerElement('object-editor', ObjectEditor);
