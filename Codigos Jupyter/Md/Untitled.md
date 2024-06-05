{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "ea2fad5b-66ba-4b47-8ca6-0b78d6824e42",
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "import math\n",
    "import cv2 as cv"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "b280a654-a507-40e8-b330-e59aab1587ce",
   "metadata": {},
   "outputs": [],
   "source": [
    "cap = cv.VideoCapture(0)\n",
    "while True:\n",
    "    ret, frame = cap.read()\n",
    "    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)\n",
    "    rostros = rostro.detectMultiScale(gray, 1.3, 5)\n",
    "    for(x, y, w, h) in rostros:\n",
    "        frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)\n",
    "        #frame2=frame[y:y+h, x:x+w]\n",
    "        #cv.imshow('rostros',frame2)\n",
    "        #frame2=cv.resize(frame2(100,100), interpolation =cv.INTER,\n",
    "        #cv.imwrite()                 \n",
    "    cv.imshow('rostros', frame)\n",
    "    k = cv.waitKey(1)\n",
    "    if k == 27:\n",
    "        break\n",
    "cap.release()\n",
    "cv.destroyAllWindows()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "27ff7fb3-f8b4-4908-a1c6-8422febc33c9",
   "metadata": {},
   "outputs": [],
   "source": [
    "C:\\Users\\david\\Desktop\\IA\\Rostros\\Pancho"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "0e37f120-6e54-43a7-8362-f02425ed68b5",
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "<>:11: SyntaxWarning: invalid escape sequence '\\R'\n",
      "<>:11: SyntaxWarning: invalid escape sequence '\\R'\n",
      "C:\\Users\\david\\AppData\\Local\\Temp\\ipykernel_6536\\1085242862.py:11: SyntaxWarning: invalid escape sequence '\\R'\n",
      "  cv.imwrite('C:\\\\Users\\\\david\\\\Desktop\\\\IA\\Rostros\\\\Pancho'+str(i)+'.png', frame2)\n"
     ]
    },
    {
     "ename": "NameError",
     "evalue": "name 'rostro' is not defined",
     "output_type": "error",
     "traceback": [
      "\u001b[1;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[1;31mNameError\u001b[0m                                 Traceback (most recent call last)",
      "Cell \u001b[1;32mIn[5], line 6\u001b[0m\n\u001b[0;32m      4\u001b[0m ret, frame \u001b[38;5;241m=\u001b[39m cap\u001b[38;5;241m.\u001b[39mread()\n\u001b[0;32m      5\u001b[0m gray \u001b[38;5;241m=\u001b[39m cv\u001b[38;5;241m.\u001b[39mcvtColor(frame, cv\u001b[38;5;241m.\u001b[39mCOLOR_BGR2GRAY)\n\u001b[1;32m----> 6\u001b[0m rostros \u001b[38;5;241m=\u001b[39m \u001b[43mrostro\u001b[49m\u001b[38;5;241m.\u001b[39mdetectMultiScale(gray, \u001b[38;5;241m1.3\u001b[39m, \u001b[38;5;241m5\u001b[39m)\n\u001b[0;32m      7\u001b[0m \u001b[38;5;28;01mfor\u001b[39;00m(x, y, w, h) \u001b[38;5;129;01min\u001b[39;00m rostros:\n\u001b[0;32m      8\u001b[0m     frame \u001b[38;5;241m=\u001b[39m cv\u001b[38;5;241m.\u001b[39mrectangle(frame, (x, y), (x\u001b[38;5;241m+\u001b[39mw, y\u001b[38;5;241m+\u001b[39mh), (\u001b[38;5;241m0\u001b[39m, \u001b[38;5;241m255\u001b[39m, \u001b[38;5;241m0\u001b[39m), \u001b[38;5;241m2\u001b[39m)\n",
      "\u001b[1;31mNameError\u001b[0m: name 'rostro' is not defined"
     ]
    }
   ],
   "source": [
    "cap = cv.VideoCapture(0)\n",
    "i = 0\n",
    "while True:\n",
    "    ret, frame = cap.read()\n",
    "    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)\n",
    "    rostros = rostro.detectMultiScale(gray, 1.3, 5)\n",
    "    for(x, y, w, h) in rostros:\n",
    "        frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)\n",
    "        frame2 = frame[y:y+h, x:x+w]\n",
    "        cv.imshow('rostro2', frame2)\n",
    "        cv.imwrite('C:\\\\Users\\\\david\\\\Desktop\\\\IA\\Rostros\\\\Pancho'+str(i)+'.png', frame2)\n",
    "    cv.imshow('rostros', frame)\n",
    "    i=i+1\n",
    "    k = cv.waitKey(1)\n",
    "    if k == 27:\n",
    "        break\n",
    "cap.release()\n",
    "cv.destroyAllWindows()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "90101143-5795-48b2-ad0f-86aaeab79ff7",
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
