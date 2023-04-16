# ngx-image-gallery
Angular 9 modal and inline image gallery

[![preview](https://i.imgur.com/1gGxBLd.jpg)](https://theorganproject.org/reinsertion)

## Prerequisites

- Hammerjs (required for swipe) 
```
npm i -S hammerjs lodash
```

Import hammerjs into your project (tip: in you main.ts file), e.g:
```
import 'hammerjs';
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
    enableProdMode();
}

document.addEventListener('DOMContentLoaded', () => {
    platformBrowserDynamic().bootstrapModule(AppModule)
        .catch(err => console.log(err));
});

```


## Install
```bash
npm install --save @codeveloped/ngx-image-gallery
```

## Import
```typescript
import { NgxImageGalleryModule } from '@codeveloped/ngx-image-gallery';

@NgModule({
  ...,
  imports: [
    NgxImageGalleryModule,
    ...
  ]
})
export class AppModule { }
```

## Use

```html
// app.component.html

<ngx-image-gallery 
[images]="images" 
[conf]="conf"
(onOpen)="galleryOpened($event)"
(onClose)="galleryClosed()"
(onImageClicked)="galleryImageClicked($event)"
(onImageChange)="galleryImageChanged($event)"
(onDelete)="deleteImage($event)"
></ngx-image-gallery>
```

## Configure
```ts
import {Component, OnInit, ViewChild} from '@angular/core';
import {NgxImageGalleryComponent, GALLERY_IMAGE, GALLERY_CONF} from '@codeveloped/ngx-image-gallery';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit {
    // get reference to gallery component
    @ViewChild(NgxImageGalleryComponent) ngxImageGallery: NgxImageGalleryComponent;

    // gallery configuration
    conf: GALLERY_CONF = {
        imageOffset: '0px',
        showDeleteControl: false,
        showImageTitle: false,
    };

    // Gallery images
    images: GALLERY_IMAGE[] = [
        {
            url: 'https://theorganproject.org/api/djZHN1JQa2RaWXVkRXNERndoeFVFZz09/7899',
            altText: 'Pipe organ restoration',
            title: 'woman-in-black-blazer-holding-blue-cup',
            thumbnailUrl: 'https://theorganproject.org/api/djZHN1JQa2RaWXVkRXNERndoeFVFZz09/7899'
        },
        {
            url: 'https://theorganproject.org/api/djZHN1JQa2RaWXVkRXNERndoeFVFZz09/7900',
            altText: 'Pipe organ restoration',
            extUrl: 'https://theorganproject.org/api/djZHN1JQa2RaWXVkRXNERndoeFVFZz09/7899,
            thumbnailUrl: 'https://theorganproject.org/api/djZHN1JQa2RaWXVkRXNERndoeFVFZz09/7899'
        },
    ];
    
    // METHODS
    // open gallery
    openGallery(index: number = 0) {
        this.ngxImageGallery.open(index);
    }

    // close gallery
    closeGallery() {
        this.ngxImageGallery.close();
    }

    // set new active(visible) image in gallery
    newImage(index: number = 0) {
        this.ngxImageGallery.setActiveImage(index);
    }

    // next image in gallery
    nextImage(index: number = 0) {
        this.ngxImageGallery.next();
    }

    // prev image in gallery
    prevImage(index: number = 0) {
        this.ngxImageGallery.prev();
    }

    /**************************************************/

    // EVENTS
    // callback on gallery opened
    galleryOpened(index) {
        console.info('Gallery opened at index ', index);
    }

    // callback on gallery closed
    galleryClosed() {
        console.info('Gallery closed.');
    }

    // callback on gallery image clicked
    galleryImageClicked(index) {
        console.info('Gallery image clicked with index ', index);
    }

    // callback on gallery image changed
    galleryImageChanged(index) {
        console.info('Gallery image changed to index ', index);
    }

    // callback on user clicked delete button
    deleteImage(index) {
        console.info('Delete image at index ', index);
    }
}
```

### Interfaces
```ts
// gallery configuration
export interface GALLERY_CONF {
  imageBorderRadius?: string; // css border radius of image (default 3px)
  imageOffset?: string; // add gap between image and it's container (default 20px)
  imagePointer? :boolean; // show a pointer on image, should be true when handling onImageClick event (default false)
  showDeleteControl?: boolean; // show image delete icon (default false)
  showCloseControl?: boolean; // show gallery close icon (default true)
  showExtUrlControl?: boolean; // show image external url icon (default true)
  showImageTitle?: boolean; // show image title text (default true)
  showThumbnails?: boolean; // show thumbnails (default true)
  closeOnEsc?: boolean; // close gallery on `Esc` button press (default true)
  reactToKeyboard?: boolean; // change image on keyboard arrow press (default true)
  reactToMouseWheel?: boolean; // change image on mouse wheel scroll (default true)
  reactToRightClick?: boolean; // disable right click on gallery (default false)
  thumbnailSize?: number; // thumbnail size (default 30)
  backdropColor?: string; // gallery backdrop (background) color (default rgba(13,13,14,0.85))
  inline?: boolean; // make gallery inline (default false)
  showArrows?: boolean; // show prev / next arrows (default true)
}

// gallery image
export interface GALLERY_IMAGE {
  url: string; // url of the image
  thumbnailUrl?: string; // thumbnail url (recommended), if not present, gallery will use `url` property to get thumbnail image.
  altText?: string; // alt text for image
  title?: string; // title of the image
  extUrl?: string; // external url of image
  extUrlTarget?: string; // external url target e.g. '_blank', '_self' etc.
}
```

> All properties ending with `?` are optional.

# Make gallery inline
You can make gallery **inline** like a carousel by setting `conf.inline` to `true` but make sure to change `conf.backdropColor` as well if you need white backdrop color. Also `width` and `height` of the gallery can be adjusted by manually applying css styles with `!important` flag on gallery element.

# Dynamic Update
You can update gallery images `images` and gallery configuration `conf` anytime you want even when gallery is opened but due to Angular's change detection restrictions you must assign these variable to new value instead of changing internal properties as mentioned below. 

```ts
// change images
this.images = this.images.concat([...]);

// change conf
this.conf = {...};
```
