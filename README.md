# Object Oriented Programming Expert With TypeScript

This repository is a complete guide and tutorial for the principles and techniques of object-oriented programming. It can be a reference for all interested in programming and software developers. You will find simple and practical examples in all sections to make the concepts easier to understand.


<p>
  <img src="https://user-images.githubusercontent.com/37804060/151722505-c16e7705-4533-48af-aebb-9d22f15c9ed0.png"/>
</p>


## Pillars of OOP

1. <strong>Abstraction</strong>
2. <strong>Encapsulation</strong>
3. <strong>Inheritance</strong>
4. <strong>Polymorphism</strong>

## SOLID Principles

<img src="https://user-images.githubusercontent.com/37804060/152614320-7c3b76d7-acdb-4266-b2c5-169fb1b4a9b3.png"/>

### 1. Single Responsibility (SRP)

> There should never be more than one reason for a class to change. Every class should have only one responsibility.

<img src="https://user-images.githubusercontent.com/37804060/152614337-e60665c6-b5b6-495b-98e0-3810a7c82542.png"/>

:x: Before following SRP:

```typescript
interface Note {
  id: string;
  text: string;
}


class Notebook {
  public readonly notes: Note[];
  private password: string;
  private theme: "LIGHT" | "DARK";
  private fontSize: number;

  constructor() {
    this.notes = [];
    this.password = "";
    this.theme = "LIGHT";
    this.fontSize = 14;
  }

  public createNewNote(text: string = ""): void {
    const newNote: Note = { id: new Date().toISOString(), text };
    this.notes.push(newNote);
  }

  public deleteAllNotes(): void {
    this.notes.length = 0;
  }

  public deleteNote(noteId: string): void {
    const targetNote = this.notes.find(({ id }) => id === noteId);
    const targetNoteIndex = this.notes.indexOf(targetNote);
    this.notes.splice(targetNoteIndex, 1);
  }

  public showNote(noteId: string): void {
    const targetNote = this.notes.find(({ id }) => id === noteId);
    console.log(targetNote.text);
  }

  public editNote(noteId: string, newText: string): void {
    const targetNote = this.notes.find(({ id }) => id === noteId);
    const targetNoteIndex = this.notes.indexOf(targetNote);
    this.notes[targetNoteIndex].text = newText;
  }

  public changePassword(newPassword: string): void {
    if (newPassword.length >= 8 && newPassword.length <= 32) {
      this.password = newPassword;
    }
  }

  public toggleTheme(): void {
    if (this.theme === "LIGHT") {
      this.theme = "DARK";
    } else {
      this.theme = "LIGHT";
    }
  }

  public changeFontSize(newFontSize: number): void {
    if (newFontSize < 8) {
      this.fontSize = 8;
    } else if (newFontSize > 60) {
      this.fontSize = 60;
    } else {
      this.fontSize = Math.floor(newFontSize);
    }
  }
}
```


:heavy_check_mark: After following SRP:

```typescript
class Note {
  public readonly id: string;
  private text: string;

  constructor(text: string) {
    this.id = new Date().toISOString();
    this.text = text;
  }

  public show(): void {
    console.log(this.text);
  }

  public edit(newText: string): void {
    this.text = newText;
  }
}

class Setting {
  private password: string;
  private theme: "LIGHT" | "DARK";
  private fontSize: number;

  constructor() {
    this.password = null;
    this.theme = "LIGHT";
    this.fontSize = 14;
  }

  private validatePassword(password: string): boolean {
    if (password.length < 8) {
      return false;
    } else if (password.length > 32) {
      return false;
    } else {
      return true;
    }
  }

  public changePassword(newPassword: string): void {
    if (this.validatePassword(newPassword)) {
      this.password = newPassword;
    }
  }

  public toggleTheme(): void {
    if (this.theme === "LIGHT") {
      this.theme = "DARK";
    } else {
      this.theme = "LIGHT";
    }
  }

  public changeFontSize(newFontSize: number): void {
    if (newFontSize < 8) {
      this.fontSize = 8;
    } else if (newFontSize > 60) {
      this.fontSize = 60;
    } else {
      this.fontSize = Math.floor(newFontSize);
    }
  }
}

class Notebook {
  public readonly notes: Note[];
  public readonly setting: Setting;

  constructor() {
    this.notes = [];
    this.setting = new Setting();
  }

  public getNoteById(noteId: string): Note | undefined {
    return this.notes.find(({ id }) => id === noteId);
  }

  public createNewNote(newNote: Note): void {
    this.notes.push(newNote);
  }

  public deleteAllNotes(): void {
    this.notes.length = 0;
  }

  public deleteNote(noteId: string): void {
    const targetNote = this.getNoteById(noteId);
    const targetNoteIndex = this.notes.indexOf(targetNote);
    this.notes.splice(targetNoteIndex, 1);
  }
}
```

### 2. Open/Closed (OCP)

> Software entities should be open for extension, but closed for modification.

<img src="https://user-images.githubusercontent.com/37804060/152614349-edd7952b-42c5-4530-b701-5f829df6dcee.png"/>

:x: Before following OCP:

```typescript
class OperatingSystemInfo {
  getFilesExtention(os: string): string {
    if (os === "Windows") {
      return "exe";
    }
    else if (os === "Linux") {
      return "deb";
    }
    else if (os === "Macintosh") {
      return "dmg";
    }
    else {
      return "unknown!";
    }
  }

  getCreator(os: string): string {
    if (os === "Windows") {
      return "Bill Gates";
    }
    else if (os === "Linux") {
      return "Linus Torvalds";
    }
    else if (os === "Macintosh") {
      return "Steve Jobs";
    }
    else {
      return "Unknown!"
    }
  }

  getBornDate(os: string): number {
    if (os === "Windows") {
      return 1985;
    }
    else if (os === "Linux") {
      return 1991;
    }
    else if (os === "Macintosh") {
      return 1984;
    }
    else {
      return -1;
    }
  }
}
```


:heavy_check_mark: After following OCP:

```typescript
interface OperatingSystemInfo {
  getFilesExtention: () => string;
  getCreator: () => string;
  getBornDate: () => number;
}

class Windows implements OperatingSystemInfo {
  getFilesExtention() {
    return "exe";
  }

  getCreator() {
    return "Bill Gates";
  }

  getBornDate() {
    return 1985;
  };
}

class Linux implements OperatingSystemInfo {
  getFilesExtention() {
    return "deb";
  }

  getCreator() {
    return "Linus Torvalds";
  }

  getBornDate() {
    return 1991;
  };
}

class Macintosh implements OperatingSystemInfo {
  getFilesExtention() {
    return "dmg";
  }

  getCreator() {
    return "Steve Jobs";
  }

  getBornDate() {
    return 1984;
  };
}
```


### 3. Liskov Substitution (LSP)

> If `S` is a subtype of `T`, then objects of type `T` in a program may be replaced with objects of type `S` without altering any of the desirable properties of that program.

<img src="https://user-images.githubusercontent.com/37804060/152614359-347f6ab7-3ffe-4a90-b3b1-c0c3ac5d8f0d.png"/>

:x: Before following LSP:

```typescript
class Tablet {
  readBook(): void {
    console.log("Enjoy reading!");
  }

  openBrowser(): void {
    console.log("Start searching ...");
  }
}

class KidsTablet extends Tablet {
  override openBrowser(): Error {
    throw Error("Kids haven't access to the browser!");
  }
}
```


:heavy_check_mark: After following LSP:

```typescript
class Tablet {
  readBook(): void {
    console.log("Enjoy reading!");
  }
}

class AdultsTablet extends Tablet {
  openBrowser(): void {
    console.log("Start searching ...");
  }
}
```


### 4. Interface Segregation (ISP)

> No code should be forced to depend on methods it does not use.

<img src="https://user-images.githubusercontent.com/37804060/152614369-1a048ac3-0d05-4429-bd0c-aad0379a6b93.png"/>

:x: Before following ISP:

```typescript
interface Ports {
  useUSB: () => void;
  useLAN: () => void;
  usePS2: () => void;
  useVGA: () => void;
}

class PC implements Ports {
  useUSB() {
    console.log("USB port is ready for your PC!");
  };

  useLAN() {
    console.log("LAN port is ready for your PC!");
  };

  usePS2() {
    console.log("PS2 port is ready for your PC!");
  };

  useVGA() {
    console.log("VGA port is ready for your PC!");
  };
}

class Laptop implements Ports {
  useUSB() {
    console.log("USB port is ready for your Laptop!");
  };

  useLAN() {
    console.log("LAN port is ready for your Laptop!");
  };

  usePS2() {
    throw new Error("Laptop has not any PS2 port!");
  };

  useVGA() {
    throw new Error("Laptop has not any VGA port!");
  };
}
```


:heavy_check_mark: After following ISP:

```typescript
interface CommonPorts {
  useUSB: () => void;
  useLAN: () => void;
}

interface ExtraPorts {
  usePS2: () => void;
  useVGA: () => void;
}

class PC implements CommonPorts, ExtraPorts {
  useUSB() {
    console.log("USB port is ready for your PC!");
  };

  useLAN() {
    console.log("LAN port is ready for your PC!");
  };

  usePS2() {
    console.log("PS2 port is ready for your PC!");
  };

  useVGA() {
    console.log("VGA port is ready for your PC!");
  };
}

class Laptop implements CommonPorts {
  useUSB() {
    console.log("USB port is ready for your Laptop!");
  };

  useLAN() {
    console.log("LAN port is ready for your Laptop!");
  };
}
```

### 5. Dependency Inversion (DIP)

> High-level modules should not import anything from low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.

<img src="https://user-images.githubusercontent.com/37804060/152614375-add2d2d6-bdbc-41d3-8b5e-82a1dfacca0d.png"/>

:x: Before following DIP:

```typescript
class TelegramApi {
  start() {
    console.log("You are connected to Telegeram API!");
  }

  messageTo(targetId: number, message: string) {
    console.log(message + " sent to " + targetId + " by Telegram!");
  }
}

class WhatsappApi {
  setup() {
    console.log("You are connected to Whatsapp API!");
  }

  pushMeesage(message: string, targetId: number) {
    console.log(message + " sent to " + targetId + " by Whatsapp!");
  }
}

class SignalApi {
  open() {
    console.log("You are connected to Signal API!");
  }

  postMessage(params: { id: number, text: string }) {
    console.log(params.text + " sent to " + params.id + " by Signal!");
  }
}

class Messenger {
  private api: TelegramApi | WhatsappApi | SignalApi;

  constructor(api: TelegramApi | WhatsappApi | SignalApi) {
    this.api = api;
  }

  sendMessage(targetId: number, message: string) {
    if (this.api instanceof TelegramApi) {
      this.api.start();
      this.api.messageTo(targetId, message);
    }
    else if (this.api instanceof WhatsappApi) {
      this.api.setup();
      this.api.pushMeesage(message, targetId);
    }
    else {
      this.api.open();
      this.api.postMessage({ id: targetId, text: message });
    }
  }
}
```


:heavy_check_mark: After following DIP:

```typescript
interface MessengerApi {
  connect: () => void;
  send: (targetId: string, message: string) => void;
}

class TelegramApi implements MessengerApi {
  connect() {
    console.log("You are connected to Telegeram API!");
  }

  send(targetId: string, message: string) {
    console.log(message + " sent to " + targetId + " by Telegram!");
  }
}

class WhatsappApi implements MessengerApi {
  connect() {
    console.log("You are connected to Whatsapp API!");
  }

  send(targetId: string, message: string) {
    console.log(message + " sent to " + targetId + " by Whatsapp!");
  }
}

class SignalApi implements MessengerApi {
  connect() {
    console.log("You are connected to Signal API!");
  }

  send(targetId: string, message: string) {
    console.log(message + " sent to " + targetId + " by Signal!");
  }
}

class Messenger {
  private api: MessengerApi;

  constructor(api: MessengerApi) {
    this.api = api;
  }

  sendMessage(targetId: string, message: string) {
    this.api.connect();
    this.api.send(targetId, message);
  }
}
```
