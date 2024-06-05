{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "9acc22a2-1d94-440d-b46e-dd016a4fcba2",
   "metadata": {},
   "outputs": [],
   "source": [
    "import cv2 as cv\n",
    "import os"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "id": "efd31d37-54d9-43d7-bb35-855a887efbb3",
   "metadata": {},
   "outputs": [],
   "source": [
    "def rota(img,i):\n",
    "    h,w = img.shape[:2]\n",
    "    mw = cv.getRotationMatrix2D((h//2, w//2),280,-1)\n",
    "    img2 = cv.warpAffine(img,mw,(h,w))\n",
    "    cv.imwrite('C://Users//david//Desktop//IA//Wally//Originnal'+str(i)+'.png',img2)\n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "id": "073a7541-ec5c-484b-89a0-48be7aa5a038",
   "metadata": {},
   "outputs": [],
   "source": [
    "i=0\n",
    "imgPaths = \"C://Users//david//Desktop//IA//Wally//Originnal\"\n",
    "nomFiles = os.listdir(imgPaths)\n",
    "for nomFiles in nomFiles:\n",
    "    i=i+1\n",
    "    imgPath=imgPaths+\"/\"+nomFiles\n",
    "    img=cv.imread(imgPath)\n",
    "    rota(img,i)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "c836a87f-756b-4000-9486-e44e198e8457",
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "2cd03cb6-609f-4936-aa34-b3ae6332b060",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.12.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
