**The game**

Exploring the game, you will find a penguin asking for a PIN

![19THM03](https://github.com/user-attachments/assets/96035f0f-493f-4944-8f37-98a888507e75)

Execute Frida to intercept all the functions in the [`libaocgame.so`](http://libaocgame.so) library where some of the game logic is present

`friday-trace ./TryUnlockMe -i 'libaocgame.so!*'`

![19SS01](https://github.com/user-attachments/assets/8c056228-3587-434d-abd4-825eaa2df19a)

After interacting with the NPC, the OTP function is being triggered and displayed as `set_otpi` [_27set_otpi]

![19SS02](https://github.com/user-attachments/assets/f3fc94ad-9f3e-49c5-a6c4-fc72248f12d4)

The i at the end of `set_otp` function indicates that an integer will be passed as a parameter. It will likely set the OTP by passing it as the first argument

To get the parameter value, use the `log` function, specifying the first elements of the array `args` on the `onEnter` function: `log("Parameter:" + args[0].toInt32());`

![19SS03](https://github.com/user-attachments/assets/9f7ff04b-65b9-4f24-8585-4e71701ed160)

![19SS04](https://github.com/user-attachments/assets/e4591948-d3be-4e55-97f5-1cb9dff795bf)

After saving the parameter, the OTP value appears. (The value changes over time.) Keying in the OTP will lead to the first flag.

![19SS05](https://github.com/user-attachments/assets/610d04a1-e8cf-4c57-9c2c-4ebbf3d30db4)

Moving on to the next stage, the game requires 1m coins to buy Flag 2. 

![19SS06](https://github.com/user-attachments/assets/76f6a8d0-57ef-4f94-9754-d59c32baf741)

The function for buying item is displayed as `_Z17validate_purchaseiii` which has three `i` letters to indicate three integer parameters

![19SS07](https://github.com/user-attachments/assets/4c6a9681-a79e-4399-ba7f-c2ca4bf77804)

Upon keying and interaction, the function gets log with the parameter. 

![19SS08](https://github.com/user-attachments/assets/2f50a446-6852-4e89-a04c-9f594a776083)

Hence, we can determine that the first parmeter is the item ID, second is the price and third is player’s coin. 

With that, we can try to manipulate the price.

![19SS09](https://github.com/user-attachments/assets/fd7fff17-62e2-434e-a06c-127b6a9a8cf5)

`args[1] = ptr(0)` : set argument index 1 (player’s coins) set to null

![19SS010](https://github.com/user-attachments/assets/6553dfbd-0293-4acb-a3e3-9b3d159d90b1)

Therefore, getting second flag.

![19SS011](https://github.com/user-attachments/assets/3911771d-38fe-4c0a-b5b2-841e1b1241b2)

Third stage.

`_Z16check_biometricsPKc()` does not handle integers, but strings

Add `log("PARAMETER:" + Memory.readCString(args[0]))` to the `OnEnter` function

![19SS012](https://github.com/user-attachments/assets/16be5a23-4d3b-4b13-8f0e-955084bbe462)

![19SS013](https://github.com/user-attachments/assets/7feeed38-155e-46e2-8c98-29e757827160)

The result is not very helpful.

adding `log("The return value is: " + retval);` to log the return value of the function

![19SS014](https://github.com/user-attachments/assets/7e2c69b4-c9fc-4da0-93ea-1f95c1da2539)

![19SS015](https://github.com/user-attachments/assets/dac87028-adea-4c7b-adc1-72d838a4f80c)

The value returned is 0, which may indicate that it is a boolean set to False. 

Replace with the following instruction `retval.replace(ptr(1))`

![19SS016](https://github.com/user-attachments/assets/d20d8d41-9a4c-4919-8105-b64f58063a4e)

With that, we get an undefined result

![19SS017](https://github.com/user-attachments/assets/c4f37a4e-f0d7-4684-9912-f2715cb8af10)

And a flag

![19SS018](https://github.com/user-attachments/assets/ceedbb82-269e-4957-8a8c-9a31cefde4c0)
