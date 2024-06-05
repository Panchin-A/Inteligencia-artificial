{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "f9eaeb6d-e607-4f86-9e2d-35e19e058582",
   "metadata": {},
   "outputs": [],
   "source": [
    "import cv2 as cv\n",
    "\n",
    "cap = cv.VideoCapture(0)\n",
    "while(True):\n",
    "    res , img = cap.read()\n",
    "    img2=cv.cvtColor(img, cv.COLOR_BGR2HSV)\n",
    "    vb=(30, 85,90)\n",
    "    va=(110, 255,255)\n",
    "    mask=cv.inRange(img2, vb,va)\n",
    "    res=cv.bitwise_and(img, img, mask=mask)\n",
    "    cv.imshow('captura', res)\n",
    "    cv.imshow('real', img)\n",
    "    if cv.waitKey(1) &0xFF == ord('s'):\n",
    "        break\n",
    "       \n",
    "cap.release()\n",
    "cv.destroyAllWindows()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "d00fdf26-55f1-4ed7-98bd-dd7f1aeab719",
   "metadata": {},
   "outputs": [],
   "source": [
    "## Tenemos que localizar la coordenaada del objeto, lo ideal es analizar la mascara"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "5d03c836-d151-4c5a-ac9d-7fe66a8b01c9",
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
