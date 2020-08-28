namespace BusinessLayer
{
    public class ApplicationBusiness
    {
        List<enumItem> lstChar;
        private Dictionary<enumItem, IPromotion> _dictPromotionCart;
        private IPromotion objPromotion;
        private PromotionDataAccess objDataAccess;

        public ApplicationBusiness()
        {
            if (_dictPromotionCart == null || _dictPromotionCart.Count <=0)
            {                  
                 objDataAccess = new PromotionDataAccess();
                 _dictPromotionCart = new Dictionary<enumItem, IPromotion>();
                _dictPromotionCart.Add(enumItem.A, new PromotionA(objDataAccess.GetPromotionData(enumItem.A)));
                _dictPromotionCart.Add(enumItem.B, new PromotionB(objDataAccess.GetPromotionData(enumItem.B)));
                _dictPromotionCart.Add(enumItem.C, new PromotionC(objDataAccess.GetPromotionData(enumItem.C)));
                _dictPromotionCart.Add(enumItem.D, new PromotionD(objDataAccess.GetPromotionData(enumItem.D)));
            }
        }


        public void AddtoCart(enumItem enumCartItem)
        {
            if (lstChar == null || lstChar.Count ==0)
            {
                lstChar = new List<enumItem>();
            }
            lstChar.Add(enumCartItem); 
        }

         
        public int CalcuateCartCost()
        {
            int TotalCost = 0;
            if (lstChar.Count > 0)
            {
                TotalCost = GetTotalCost(enumItem.A);
                TotalCost += GetTotalCost(enumItem.B);

                if (lstChar.Contains(enumItem.C) && lstChar.Contains(enumItem.D)) 
                {
                    int TotalCCount = lstChar.Count(ListItem => ListItem == enumItem.C);
                    int TotalDCount = lstChar.Count(ListItem => ListItem == enumItem.D);
                    GetTotalCost(enumItem.D);
                    if (TotalCCount == TotalDCount)
                    {
                        TotalCost += TotalCCount * objPromotion.PromotionPrice;
                    }
                    else if (TotalCCount > TotalDCount)
                    {
                        TotalCost += TotalDCount * objPromotion.PromotionPrice;
                        GetTotalCost(enumItem.C);
                        TotalCost += (TotalCCount - TotalDCount) * objPromotion.UnitPrice;
                    }
                    else
                    {
                        TotalCost += TotalCCount * objPromotion.PromotionPrice;
                        TotalCost += (TotalDCount - TotalCCount) * objPromotion.UnitPrice;
                    }                    
                }
                else
                {
                    TotalCost += GetTotalCost(enumItem.C);
                    TotalCost += GetTotalCost(enumItem.D);
                }
            }
            if (lstChar != null && lstChar.Count > 0 )
            {
                lstChar.Clear();
            }

            return TotalCost;
        }


        private int GetTotalCost(enumItem enumValue)
        {
            _dictPromotionCart.TryGetValue(enumValue, out objPromotion);
            return objPromotion.GetCostPrice(lstChar.Count(ListItem => ListItem == enumValue));
        }
    }
}


namespace BusinessLayer
{
    interface IPromotion
    {
        int UnitPrice { get; set; }
        int DiscountUnitCount { get; set; }
        int PromotionPrice { get; set; }

        int GetCostPrice(int CartUnits);
    }
}

namespace BusinessLayer
{
    internal class PromotionA : IPromotion
    {
        public int UnitPrice { get; set; }
        public int DiscountUnitCount { get; set; }
        public int PromotionPrice { get; set; }

        public PromotionA(stPromotion pobjPromotion)
        {
            this.UnitPrice = pobjPromotion.UnitPrice;
            this.DiscountUnitCount = pobjPromotion.DiscountUnits;
            this.PromotionPrice = pobjPromotion.DiscountedPrice ;
        }

        public int GetCostPrice(int CartUnits)
        {
            int _intTotalPrice = 0;

            if (DiscountUnitCount > CartUnits)
            {
                _intTotalPrice = CartUnits * this.UnitPrice;
            }
            else
            {
                int Quotient = CartUnits / DiscountUnitCount;
                int Remainder = CartUnits % DiscountUnitCount;

                _intTotalPrice += Remainder * UnitPrice + Quotient * this.PromotionPrice;
            }

            return _intTotalPrice;
        }
    }
}

namespace BusinessLayer
{
    class PromotionB : IPromotion
    {
        public int UnitPrice { get; set; }
        public int DiscountUnitCount { get; set; }
        public int PromotionPrice { get; set; }

        public PromotionB(stPromotion pobjPromotion)
        {
            this.UnitPrice = pobjPromotion.UnitPrice;
            this.DiscountUnitCount = pobjPromotion.DiscountUnits;
            this.PromotionPrice = pobjPromotion.DiscountedPrice;
        }

        public int GetCostPrice(int CartUnits)
        {
            int _intTotalPrice = 0;

            if (DiscountUnitCount > CartUnits)
            {
                _intTotalPrice = CartUnits * this.UnitPrice;
            }
            else
            {
                int Quotient = CartUnits / DiscountUnitCount;
                int Remainder = CartUnits % DiscountUnitCount;

                _intTotalPrice += Remainder * UnitPrice + Quotient * this.PromotionPrice;
            }

            return _intTotalPrice;
        }
    }

}



namespace BusinessLayer
{
    class PromotionB : IPromotion
    {
        public int UnitPrice { get; set; }
        public int DiscountUnitCount { get; set; }
        public int PromotionPrice { get; set; }

        public PromotionB(stPromotion pobjPromotion)
        {
            this.UnitPrice = pobjPromotion.UnitPrice;
            this.DiscountUnitCount = pobjPromotion.DiscountUnits;
            this.PromotionPrice = pobjPromotion.DiscountedPrice;
        }

        public int GetCostPrice(int CartUnits)
        {
            int _intTotalPrice = 0;

            if (DiscountUnitCount > CartUnits)
            {
                _intTotalPrice = CartUnits * this.UnitPrice;
            }
            else
            {
                int Quotient = CartUnits / DiscountUnitCount;
                int Remainder = CartUnits % DiscountUnitCount;

                _intTotalPrice += Remainder * UnitPrice + Quotient * this.PromotionPrice;
            }

            return _intTotalPrice;
        }
    }
}

namespace BusinessLayer
{
    
    class PromotionC : IPromotion
    {
        public int UnitPrice { get; set; }
        public int DiscountUnitCount { get; set; }
        public int PromotionPrice { get; set; }

        public PromotionC(stPromotion pobjPromotion )
        {
            this.UnitPrice = pobjPromotion.UnitPrice;
            this.DiscountUnitCount = pobjPromotion.DiscountUnits ;
            this.PromotionPrice =  pobjPromotion.DiscountedPrice;
        }

        public int GetCostPrice(int CartUnits)
        {
            int _intTotalPrice = 0;

            if (DiscountUnitCount >= CartUnits)
            {
                _intTotalPrice = CartUnits * this.UnitPrice;
            }
            else
            {
                int Quotient = CartUnits / DiscountUnitCount;
                int Remainder = CartUnits % DiscountUnitCount;

                _intTotalPrice += Remainder * UnitPrice + Quotient * this.PromotionPrice;
            }

            return _intTotalPrice;
        }
    }

}

namespace BusinessLayer
{
    
    class PromotionD : IPromotion
    {
        public int UnitPrice { get; set; }
        public int DiscountUnitCount { get; set; }
        public int PromotionPrice { get; set; }

        public PromotionD(stPromotion stPromotion)
        {
            this.UnitPrice =            stPromotion.UnitPrice;
            this.DiscountUnitCount =    stPromotion.DiscountUnits ;
            this.PromotionPrice =       stPromotion.DiscountedPrice;
        }

        public int GetCostPrice(int CartUnits)
        {
            int _intTotalPrice = 0;

            if (DiscountUnitCount >= CartUnits)
            {
                _intTotalPrice = CartUnits * this.UnitPrice;
            }
            else
            {
                int Quotient = CartUnits / DiscountUnitCount;
                int Remainder = CartUnits % DiscountUnitCount;

                _intTotalPrice += Remainder * UnitPrice + Quotient * this.PromotionPrice;
            }

            return _intTotalPrice;
        }
    }

}

namespace CommonLayer
{      
    public enum enumItem : byte
    {
        A,
        B,
        C,
        D,
    } 
    public struct stPromotion
    {
        public enumItem enumItem;
        public int UnitPrice;
        public int DiscountUnits;
        public int DiscountedPrice;
    }

}

namespace DataAccessLayer
{
    public class PromotionDataAccess
    {
        List<stPromotion> lstPromotion;

        public stPromotion GetPromotionData(enumItem enumItem)
        {
            if (lstPromotion == null || lstPromotion.Count <= 0)
            {
                lstPromotion = new List<stPromotion>();
                lstPromotion.Add(new stPromotion() { enumItem = enumItem.A, UnitPrice = 50, DiscountUnits = 3, DiscountedPrice = 130 });
                lstPromotion.Add(new stPromotion() { enumItem = enumItem.B, UnitPrice = 30, DiscountUnits = 2, DiscountedPrice = 45 });
                lstPromotion.Add(new stPromotion() { enumItem = enumItem.C, UnitPrice = 20, DiscountUnits = 1, DiscountedPrice = 30 });
                lstPromotion.Add(new stPromotion() { enumItem = enumItem.D, UnitPrice = 15, DiscountUnits = 1, DiscountedPrice = 30 });
            }
            return lstPromotion.Find(listItem => listItem.enumItem == enumItem);
        }
    }
}

namespace PromotionEngine
{
    class Program
    {
        static void Main(string[] args)
        {
           ApplicationBusiness objBusiness = new ApplicationBusiness();

           /// Scenario A
           /// 1 * A    50
           /// 1 * B    30
           /// 1 * C    20
           /// Total 100

            objBusiness.AddtoCart(enumItem.A);  // Add One Char A
            objBusiness.AddtoCart(enumItem.B);  // Add One Char B          
            objBusiness.AddtoCart(enumItem.C);  // Add One Char C

            int cartCost =  objBusiness.CalcuateCartCost(); // Calulcate the Total Cart Price
            Console.WriteLine("Scenario A Items :");
            Console.WriteLine("1 * A");
            Console.WriteLine("1 * B");
            Console.WriteLine("1 * C");

            Console.WriteLine("Scenario A Total Cart Cost :" + cartCost);
            Console.WriteLine("-----------------------------------------------------Press Enter To Continue----------------");
            Console.ReadKey();

            /// Scenario B
            /// 5 * A    130 + 2 * 50
            /// 5 * B    45 + 45 + 30
            /// 1 * C    20
            /// Total 370

            objBusiness.AddtoCart(enumItem.A);  // Add Five Char A
            objBusiness.AddtoCart(enumItem.A);
            objBusiness.AddtoCart(enumItem.A);
            objBusiness.AddtoCart(enumItem.A);
            objBusiness.AddtoCart(enumItem.A);

            objBusiness.AddtoCart(enumItem.B);  // Add Five Char B
            objBusiness.AddtoCart(enumItem.B);
            objBusiness.AddtoCart(enumItem.B);
            objBusiness.AddtoCart(enumItem.B);
            objBusiness.AddtoCart(enumItem.B);

            objBusiness.AddtoCart(enumItem.C);  // Add Five Char C

            cartCost = objBusiness.CalcuateCartCost();

            Console.WriteLine("Scenario B Items :");
            Console.WriteLine("5 * A");
            Console.WriteLine("5 * B");
            Console.WriteLine("1 * C");
            Console.WriteLine("Scenario B: Total Cart Cost :" + cartCost);
            Console.WriteLine("-----------------------------------------------------Press Enter To Continue----------------");
            Console.ReadKey();

            /// Scenario C
            /// 3 * A    130 + 2 * 50
            /// 5 * B    45 + 45 + 30
            /// 1 * C    -
            /// 1 * D    - 30
            /// Total 280

            objBusiness.AddtoCart(enumItem.A);   // Add Three Char A
            objBusiness.AddtoCart(enumItem.A);
            objBusiness.AddtoCart(enumItem.A);


            objBusiness.AddtoCart(enumItem.B);    // Add Five Char B
            objBusiness.AddtoCart(enumItem.B);
            objBusiness.AddtoCart(enumItem.B);
            objBusiness.AddtoCart(enumItem.B);
            objBusiness.AddtoCart(enumItem.B);

            objBusiness.AddtoCart(enumItem.C);    // Add One Char C  

            objBusiness.AddtoCart(enumItem.D);   // Add One Char D

            cartCost = objBusiness.CalcuateCartCost();
            Console.WriteLine("Scenario B Items :");
            Console.WriteLine("3 * A");
            Console.WriteLine("5 * B");
            Console.WriteLine("1 * C");
            Console.WriteLine("1 * C");
            Console.WriteLine("Scenario C: Total Cart Cost :" + cartCost);
            Console.WriteLine("-----------------------------------------------------Press Enter To Exit ----------------");
            Console.ReadKey();
        }
    }
}

